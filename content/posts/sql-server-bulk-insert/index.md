+++
title = "How to Insert Millions of Records into SQL Server"
date = 2025-04-10T10:00:00-03:00
description = "Discover how to efficiently insert millions of records into a SQL Server table using BULK INSERT and TABLOCK."
draft = false
tags = ["SQL Server", "Database", "Performance", "Bulk Insert", "SQL", "Data Engineering", "LinkedIn Post"]
categories = ["Database", "Performance"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "A code snippet illustrating the syntax for BULK INSERT in SQL Server"
    caption = "Optimizing large-scale data ingestion in SQL Server using Bulk Insert and Tablock"
    relative = true
+++

**How to insert millions of records into a SQL Server table?**

A few days ago, I was asked about it.
It's funny how a tool you use usually can slip your mind.

Anyway, `BULK INSERT` is a very efficient resource for inserting millions of records. It can increase the insertion speed by up to 10x to 50x, which means that it can significantly reduce the time required for the operation.

When used together with `TABLOCK`, the performance is even better, as SQL Server locks the entire table during the operation. This avoids transaction concurrency and the excess of logs, and avoids the need to manage numerous smaller locks.

### Key parameters for BULK INSERT include:
* **FIELDTERMINATOR:** Defines the character that separates the columns in the text file.
* **ROWTERMINATOR:** Defines the character that separates the lines (usually `\n`, representing the line break).
* **BATCHSIZE:** Defines how many records will be processed per batch.

**Have you already used BULK INSERT?**

---

👉 *Adapted from my original post on [LinkedIn](https://www.linkedin.com/posts/potumati_how-to-insert-millions-of-records-into-a-activity-7293240777114058753-aOZj?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAAAGzxU4BM2J38YwZLdJjXXQDdoPvNE5m_d0).*
