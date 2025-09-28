---
title: 'Devlog #6 MyLife.NET — Let us go mobile with MAUI'
date: '2024-10-28'
---

Life—and _[MyLife.NET](https://github.com/tscholze/dotnet-mylife)_—no longer happens only on the web or in the cloud; it’s mobile too—and often primarily there. _[.NET MAUI](https://learn.microsoft.com/de-de/dotnet/maui/what-is-maui?view=net-maui-8.0)_ provides a great way for developers in this ecosystem to build apps for Apple, Android, and Windows simultaneously.

Community FTW!
--------------

Even if I repeat myself: a helpful, educational community around whatever I’m diving into is essential if I’m going to invest my free time. That starts with the great community around Dr. Windows and extends to programming languages, 3D printing, and other exciting hobbies to spend your evenings on.

With _.NET MAUI_ this isn’t as pronounced as with mainstream frameworks like React Native, but I still wholeheartedly recommend two Discord communities.

### MAUI Community ToolKit Community

This small but excellent [English-speaking community](https://discord.gg/5h9jzdHP) is the hangout for the people behind the powerful MAUI Community Toolkit (MCT) NuGet package. Questions from beginners about _MAUI_ are answered here with real expertise. Ecosystem heavyweights like Gerald J are also there, chatting with everyone—maintainers, pros, and beginners alike. A genuinely relaxed environment.

### C# Discord Server

As the name suggests, this one has a broader scope. [This server](https://discord.gg/csharp) covers, in separate channels, nearly everything you can do with _C#_ and _.NET_. The “mobile” channel, in turn, specifically concerns mobile app development; it’s not limited to _MAUI_ but from time to time also covers other frameworks like Uno or Avalonia.

My mistake, MAUI’s bug, or who’s to blame?
------------------------------------------

That’s not always easy for me to say. While building toward the app’s current state, I often felt like I was running into walls and couldn’t move forward or backward because some function or feature didn’t work the way I’d sketched it in my head. Even when those communities could help or point me to _.NET MAUI_ GitHub issues, I was—and still am—often unsure whether the problem is really with MAUI or rather with my inexperience, because these were often “this should just work” kinds of things.

One example is setting a border color in XAML via a binding. In the following use case, I wanted to set the color programmatically.

The binding for the corner radius worked immediately without any issues as soon as I wrote it. The binding below for the border color did not. I reasoned a lot about control hierarchy and such—XAML can sometimes become very complex there.

On GitHub you’ll find the issue “_[TemplateBinding with CornerRadius on Border does not work anymore #21747](https://github.com/dotnet/maui/issues/21747)_,” where after some back and forth it became clear that it was a .NET MAUI problem, and it has now been fixed in a new version of the framework.

Either way—it’s just nice to finally see the alternating border color feature working.

TIL: XAML Converter vs. Wrapped Model
-------------------------------------

Behind this perhaps cryptic-sounding question is a point I’d never considered in my XAML life so far: when should you use a converter ([Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/maui/fundamentals/data-binding/converters?view=net-maui-8.0)) in XAML, and when should a custom component wrap the data model into another model that also contains, for example, styling parameters like the radius and the border color from the example above?

I don’t think this question has a simple answer. For me, I’ve found that as soon as there are different hierarchy levels to parameterize in XAML, or converters need to iterate over a data source, the wrapped-model route is easier to develop and faster at runtime than the now rather old-fashioned converter classes.

What do you think? How do you use converters when it’s about more than just formatting text? Let’s chat!

Visual Studio Code keeps getting better—still meh
--------------------------------------------------

Microsoft is working hard to make Visual Studio Code a full-fledged development environment for .NET projects. In my experience it’s getting better, especially regarding MAUI and XAML. So I really see VSC as the tool of choice in the future when it comes to doing anything with C# or .NET—across all platforms, whether Windows or, in my case, macOS. The emphasis here is on “in the future”; right now it’s still no match for Visual Studio or JetBrains Rider.

As a Mac user who does .NET as a hobby, that leaves me choosing between a less comfortable editor or paying the annual subscription for [JetBrains Rider](https://www.jetbrains.com/de-de/rider/)—unless they happen to offer a free EAP (Early Access Program) version, which, as the name suggests, can be quite buggy.

What’s next?
------------

If you take a look at the current state, you’ll notice everything is still pretty bare-bones. Clicking the post tiles doesn’t do anything yet, and the app feels empty otherwise. I’ll gradually bring it to life and evaluate where certain user interactions make sense. I’ve also thought about recreating this state in [_MAUI Hybrid_](https://learn.microsoft.com/de-de/aspnet/core/blazor/hybrid/tutorials/maui?view=aspnetcore-8.0) with Blazor—if only we had five-day weekends.

Even though the project’s progress has stalled for a variety of reasons, that doesn’t mean I’ve given up. I enjoy the freedom of its status as a “tinkering project,” so I don’t have to stress about implementing features; I can sit down at the computer when I’m in the mood to work on feature X in MyLife.NET.

Want to get involved?
---------------------

If you’d like to participate—because you want to show me how to do something correctly or more simply, or you want to contribute to a still-manageable .NET solution for the first time—you’re warmly invited to swing by GitHub, fork the repository, open issues with problems or pointers, or just browse. We’re all here to learn and, even more importantly, to have fun with software development!
