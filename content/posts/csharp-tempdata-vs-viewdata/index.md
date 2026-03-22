+++
title = "TempData in ASP.NET Core: Transferring Data Between Controllers"
date = 2024-08-15T10:00:00-03:00
description = "Understanding when and how to use TempData for transferring data in ASP.NET Core applications."
draft = false
tags = ["csharp", "dotnet", "Software Engineering", "ASP.NET", "dotnet-core", "ASP.NET Core", "ASP.NET MVC", "LinkedIn Post"]
categories = ["Backend", "Tips", "Web Development"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "Code snippet illustrating data transfer between controllers using TempData"
    caption = "Using TempData to safely transfer data across requests in ASP.NET Core"
    relative = true
+++

**TempData - Transfer data between controllers**

Ideal for transferring small amounts of data between controllers in ASP.NET MVC.

Unlike `ViewData` and `ViewBag`, which are specific to a single view, `TempData` persists for one request cycle.

This means the data remains available until it's accessed, at which point it's automatically removed.

---

👉 *Adapted from my original post on [LinkedIn](https://www.linkedin.com/posts/potumati_csharp-dotnet-softwareengineering-activity-7206626292559945729-OFrA?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAAAGzxU4BM2J38YwZLdJjXXQDdoPvNE5m_d0).*
