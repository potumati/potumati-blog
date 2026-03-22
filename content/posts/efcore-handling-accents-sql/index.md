+++
title = "Handling Accents in SQL with EF Core"
date = 2024-03-15T10:00:00-03:00
description = "Learn how to use EF.Functions.Collate to handle accents when querying SQL Server using Entity Framework Core, along with the performance implications."
draft = false
tags = ["csharp", "dotnet", "dotnet-core", "EF", "EF Core", "SQL", "SQL Server", "LinkedIn Post"]
categories = ["Backend", "Database"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "Code snippet demonstrating EF.Functions.Collate for accent-insensitive querying in EF Core"
    caption = "Properly handling accents in SQL Server using EF.Functions.Collate"
    relative = true
+++

**Have you ever needed to handle accents in SQL?**

If you are Latin, you have certainly done it many times.

And how does it work in EF Core?

It's easy, you just have to use `EF.Functions.Collate`!

> **Warning:** Overriding case-sensitivity in a query via `EF.Functions.Collate` (or by calling `string.ToLower`) can have a very significant impact on your application's performance.

🔗 [Know more](https://lnkd.in/dMs3HnVS)

---

👉 *Adapted from my original post on [LinkedIn](https://www.linkedin.com/posts/potumati_csharp-dotnet-netcore-activity-7178375191343874048-NJr8?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAAAGzxU4BM2J38YwZLdJjXXQDdoPvNE5m_d0).*
