+++
title = "List<T> or IEnumerable<T> in C#: Which one to choose?"
date = 2025-11-15T10:00:00-03:00
description = "A quick guide on understanding when to use IEnumerable<T>, ICollection<T>, IList<T>, and List<T> in C# for better software architecture."
draft = false
tags = ["dotnet", "csharp", "Programming", "Software Development", "Clean Architecture", "Coding Tips", "LinkedIn Post"]
categories = ["Backend", "Architecture"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "A comparison table or code snippet demonstrating List vs IEnumerable in C#"
    caption = "Understanding when to use the minimal required interface in C#"
    relative = true
+++

That classic question: `List<T>` or `IEnumerable<T>` in C#?

Straight to the point:

* **`IEnumerable<T>`**: when you only need forward iteration. It also works with deferred execution, the basis of LINQ and ideal for handling large amounts of data without loading everything into memory.
* **`ICollection<T>`**: in addition to iteration, it allows you to add, remove, and count items, but without index access.
* **`IList<T>`**: includes everything from `ICollection<T>`, plus index-based access and the ability to insert/remove at specific positions.
* **`List<T>`**: the concrete implementation of `IList<T>`, the class you usually use to instantiate a list.

### Why does this matter?

Whenever possible, use the minimal required interface.
Clear contracts reduce coupling and make your code more flexible and easier to maintain.

---

👉 *Adapted from my original post on [LinkedIn](https://www.linkedin.com/posts/potumati_dotnet-csharp-programming-activity-7379141795953152001-FNYK?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAAAGzxU4BM2J38YwZLdJjXXQDdoPvNE5m_d0).*
