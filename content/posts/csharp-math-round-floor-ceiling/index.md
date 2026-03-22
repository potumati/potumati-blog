+++
title = "Differences Between Round, Floor, and Ceiling in C#"
date = 2024-02-15T10:00:00-03:00
description = "Understanding how Math.Round, Math.Floor, and Math.Ceiling behave differently when handling decimals in C#."
draft = false
tags = ["csharp", "dotnet-core", "Developer", "Math", "dotnet", "Full-Stack Developer", "LinkedIn Post"]
categories = ["Backend", "Tips", "Clean Code"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "Code snippet demonstrating the differences between Round, Floor, and Ceiling in C#"
    caption = "Choosing the correct mathematical rounding function in C#"
    relative = true
+++

**Do you know the differences between Round, Floor, and Ceiling in C#?**

* **Round:** It follows standard mathematical rounding to the nearest integer.  
  For example, `5.67 => 6`, and `5.47 => 5`.

* **Floor:** It rounds the number *down* to the nearest integer.  
  For example, `5.67 => 5`, and `5.47 => 5`.

* **Ceiling:** It rounds the number *up* to the nearest integer.  
  For example, `5.67 => 6`, and `5.47 => 6`.

---

👉 *Adapted from my original post on [LinkedIn](https://www.linkedin.com/posts/potumati_csharp-netcore-developer-activity-7164321756680335360-ZYIj?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAAAGzxU4BM2J38YwZLdJjXXQDdoPvNE5m_d0).*
