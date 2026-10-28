+++
title = "TempDB Spill: The Silent Server Killer"
date = 2026-08-18T10:00:00-03:00
description = "A real case of a geospatial query spilling to TempDB - why memory grants lie, how PerfMon exposed it, and the approach I follow to find the root cause."
draft = false
slug = "sql-server-tempdb-spill"
tags = ["SQL Server", "Database", "Performance", "TempDB", "Query Tuning", "Execution Plan", "SQL"]
categories = ["Database", "Performance"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "SQL Server execution plan showing a Sort operator spilling to TempDB"
    caption = "TempDB spills are rarely the cause - they're the symptom"
    relative = true
+++

**Perfect index. Solid execution plan. You've reviewed the query line by line. And yet, sometimes everything crashes.**

If all basic metrics look normal… no CPU spikes, no memory pressure, no network bottleneck. You may be a spill-to-TempDB victim.

## Ok, but what is a TempDB spill?

Some SQL Server operations, like Sort (especially with geospatial queries - my experience) and Hash Match, require memory. Before running a query, SQL Server estimates how much memory it needs.

Sometimes that estimate is underestimated. When there isn't enough memory available, SQL Server writes part of the data to TempDB instead of keeping it in memory.

---

## The case

Recently, on [adotar.com.br](https://adotar.com.br), I had a geospatial query with a `Sort` operator estimating a few thousand rows... **actual: hundreds of thousands.**

The Sort spilled to TempDB. Every single execution during peak hours, injecting gigabytes of data into TempDB.

The big issue: it was causing problems on another project running on the same server. I checked all the basic metrics - no memory overuse, no CPU spikes, no network bottleneck. Everything looked fine.

I only found the issue when I started watching PerfMon counters, specifically **Avg. Disk Queue Length** was getting bigger on the SQL Server's drive, so a fast look at TempDB was enough.

Both (disk queue and TempDB) told a story the CPU and memory counters didn't.

---

## Root cause

The estimate was based on a bad assumption for that specific filter combination.

No amount of index tuning fixed it, because the index wasn't the problem - the query plan's memory grant was.

Rewrote it using a CTE to break up the geospatial filtering and force a better estimate earlier in the plan. **Spill gone.**

The index was fine. The query "looked" fine. But nobody had checked if the memory grant matched reality.

---

## To be practical

So keep in mind: TempDB spills, usually, are not the cause, but a symptom. There's no script to follow to solve it - you need to discover the root cause.

I usually follow this approach:

1. **Rewrite the query:** restructure complex logic (or use temporary tables/CTEs strategically);
2. **Check statistics and parameter sniffing:** old stats are the primary culprit behind poor memory grants;
3. **Create a better index:** if missing columns are inflating the row width being sorted;
4. **Increase available memory:** only as a last resort.

SQL Server 2019+, with **Memory Grant Feedback**, can reduce or solve the problem, but the main goal is to understand *why* the spill happened in the first place.

---

## A look at the TempDB

### TempDB usage by session / active work

**Good to find the culprit in real time:**

```sql
SELECT 
    s.session_id,
    r.status,
    r.command,
    t.text AS query_text,
    tsu.user_objects_alloc_page_count * 8 / 1024.0 AS user_objects_mb,
    tsu.internal_objects_alloc_page_count * 8 / 1024.0 AS internal_objects_mb, -- spills show up here
    (tsu.user_objects_alloc_page_count + tsu.internal_objects_alloc_page_count) * 8 / 1024.0 AS total_mb
FROM sys.dm_db_task_space_usage tsu
JOIN sys.dm_exec_sessions s ON tsu.session_id = s.session_id
JOIN sys.dm_exec_requests r ON tsu.session_id = r.session_id
CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) t
WHERE tsu.internal_objects_alloc_page_count > 0
ORDER BY internal_objects_mb DESC;
```

### Identifying which queries are causing the spill

```sql
SELECT 
    r.session_id,
    r.status,
    t.text AS query_text,
    p.query_plan
FROM sys.dm_exec_requests r
CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) t
CROSS APPLY sys.dm_exec_query_plan(r.plan_handle) p
WHERE r.session_id IN (
    SELECT session_id 
    FROM sys.dm_db_task_space_usage 
    WHERE internal_objects_alloc_page_count > 0
);
```
