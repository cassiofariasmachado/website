---
title: "System.HashCode: It's never been easier to implement GetHashCode method"
author: "Cássio"
authorAvatarPath: "/profile.jpeg"
date: "2018-09-17"
summary: "Overview about using System.HashCode to simplify GetHashCode implementations in .NET Core."
description: "Overview about using System.HashCode to simplify GetHashCode implementations in .NET Core."
toc: false
readTime: true
autonumber: false
math: false
tags: ["csharp", "dotnet"]
showTags: true
hideBackToTop: false
fediverse: "@cassiofariasmachado@mastodon.social"
---

Who has never lost good minutes searching the internet when they had to override the `GetHashCode` method? I have!

It was quite common to find confusing implementations like this one:

```csharp
public class Person
{
    public int Id { get; set; }

    public string Name { get; set; }

    public override bool Equals(object obj) => // ...

    public override int GetHashCode()
    {
        int hash = 13;

        hash = (hash * 7) + Id.GetHashCode();
        hash = (hash * 7) + Name.GetHashCode();

        return hash;
    }
}
```

Really, it wasn't an easy task to choose an implementation that would generate a good hash. That's right, it wasn't! Since .NET Core version 2.1, a new `struct` called `System.HashCode` has been added, which makes implementing the method much — much — easier.

The `Person` class, presented earlier, can now be implemented like this:

```csharp
using System;

public class Person
{
    public int Id { get; set; }

    public string Name { get; set; }

    public override bool Equals(object obj) => // ...

    public override int GetHashCode() => HashCode.Combine(Id, Name);
}
```

![Deadpool surprised with the System.HashCode struct](static/posts/2019/system-hashcode/suprised-deadpool.gif)

Much easier, right!?

The static `Combine` method from the example can generate hashes by combining up to eight parameters. This is the simplest way to use the new API. However, it also provides instance methods that can be used to generate your hashes, such as `ToHashCode`. You can check more examples in the [official documentation](https://docs.microsoft.com/en-gb/dotnet/api/system.hashcode) of the `struct`.

The proposal for this API started being discussed in [this issue](https://github.com/dotnet/corefx/issues/14354) on the [CoreFx](https://github.com/dotnet/corefx) GitHub, in December 2016, and was released with .NET Core version 2.1 on May 30, 2018. It's also available through the NuGet package [Microsoft.Bcl.HashCode](https://www.nuget.org/packages/Microsoft.Bcl.HashCode) for earlier Framework versions, but it's still in _preview_.

So what do you think of this new feature? I really liked it! It's always good when we reduce the complexity of routine implementations and can focus on what really matters: our system's business rules.

### References

- [Proposal: Add System.HashCode to make it easier to generate good hash codes](https://github.com/dotnet/corefx/issues/14354)
- [Easier GetHashCode implementation in .NET Core 2.1](https://www.tabsoverspaces.com/233725-easier-gethashcode-implementation-in-net-core-2-1)
- [System.HashCode](https://docs.microsoft.com/en-gb/dotnet/api/system.hashcode)
