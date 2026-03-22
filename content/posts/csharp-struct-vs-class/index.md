+++
title = "Struct or Class: When to Use Which in C#"
date = 2023-12-15T10:00:00-03:00
description = "A practical guide to help you choose between structs and classes when defining new types in C#."
draft = false
tags = ["Coding Tips", "Programming", "csharp", "dotnet-core", "dotnet", "Developer", "LinkedIn Post"]
categories = ["Backend", "Software Architecture"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "Code diagram comparing struct and class memory allocation in .NET"
    caption = "Making the right choice between Reference and Value types in C#"
    relative = true
+++

**Struct or class?**

Most types in a framework should be classes, but there are some situations in which the characteristics of a value type make it more appropriate to use structs.

Here are some things to consider:

👍 If your instances are small and short-lived, or commonly embedded in other objects, consider using a `struct`. 

👎 However, be cautious when defining a `struct`. Only use it if the type represents a single value, is immutable, has an instance size under 16 bytes, and will not need to be boxed frequently. 

Keep it in mind when defining new types in your code, and you'll be able to make the best choice between a `class` or a `struct`. 

---

👉 *Adapted from my original post on [LinkedIn](https://www.linkedin.com/posts/potumati_codingtips-programming-csharp-activity-7163139782976516096-SKPn?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAAAGzxU4BM2J38YwZLdJjXXQDdoPvNE5m_d0).*
