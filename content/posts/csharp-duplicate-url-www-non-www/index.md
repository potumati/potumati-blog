+++
title = "Handling Duplicate URLs in ASP.NET MVC: www vs non-www"
date = 2024-07-15T10:00:00-03:00
description = "Learn how to efficiently manage www and non-www domains in ASP.NET MVC using the UseRewriter middleware."
draft = false
tags = ["csharp", "dotnet", "Software Engineering", "ASP.NET", "dotnet-core", "SEO", "LinkedIn Post"]
categories = ["Backend", "Tips", "Web Development"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "Code snippet illustrating the UseRewriter middleware in ASP.NET Core"
    caption = "Handling WWW and non-WWW domains correctly using UseRewriter"
    relative = true
+++

**Concerned about duplicate URLs in ASP.NET MVC?**

Take care of both `www` and `non-www` domains. .NET already provides the correct method to handle them. 

Using the `UseRewriter` middleware is the optimal approach.

You can use `AddRedirectToNonWww()` or `AddRedirectToWww()` for this purpose.

---

👉 *Adapted from my original post on [LinkedIn](https://www.linkedin.com/posts/potumati_csharp-dotnet-softwareengineering-activity-7196487003713028096-fv21?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAAAGzxU4BM2J38YwZLdJjXXQDdoPvNE5m_d0).*
