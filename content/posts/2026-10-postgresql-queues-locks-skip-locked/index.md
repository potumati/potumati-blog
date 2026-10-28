+++
title = "PostgreSQL: Dealing with Queues and Locks"
date = 2026-10-28T10:00:00-03:00
description = "A quick look at FOR UPDATE SKIP LOCKED for safely consuming PostgreSQL queue rows with multiple concurrent workers."
draft = false
slug = "postgresql-queues-locks-skip-locked"
tags = ["PostgreSQL", "Database", "Concurrency", "Queues", "Outbox Pattern", "SQL"]
categories = ["Database", "Backend"]
author = "Eduardo Potumati"
[cover]
    image = "cover.png"
    alt = "Multiple workers consuming different rows from a PostgreSQL queue while skipping a locked row"
    caption = "FOR UPDATE SKIP LOCKED lets concurrent workers claim jobs without blocking each other"
    relative = true
+++

When we're working with multiple services or workers consuming the same queue table, we'll probably have problems with one worker locking a row and blocking another.

Or, even worse, both processing the same row.

**PostgreSQL has the solution!**

*(Ok, to be fair, SQL Server has it too `WITH (UPDLOCK, ROWLOCK, READPAST)`, but let's keep our focus on PostgreSQL, which I'm using this week)*

{{< figure src="postgresql-queues-locks-skip-locked-linkedin.jpg" alt="PostgreSQL queue processing with FOR UPDATE SKIP LOCKED" caption="Concurrent workers can skip rows that are already locked and claim the next available job" >}}

```sql
BEGIN;

SELECT id, payload
FROM job_queue
WHERE status = 'pending'
ORDER BY created_at
LIMIT 1
FOR UPDATE SKIP LOCKED;

-- Process your job here

UPDATE job_queue
SET status = 'completed'
WHERE id = :id;

COMMIT;
```

`FOR UPDATE` locks the selected row for the current transaction.

`SKIP LOCKED` tells PostgreSQL: if another worker has already locked this row, don't wait, just keep going and find the next available one.

This allows multiple workers to consume jobs concurrently without waiting for each other and without claiming the same row.

So, keep it in mind when you're implementing your outbox pattern solution.

**Always make sure it runs inside an explicit transaction (`BEGIN`/`COMMIT`), or the lock will be released immediately.**
