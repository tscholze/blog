---
title: 'Devlog #7 MyLife.NET - 2025 will Be the Year of MAUI'
date: '2025-01-31'
---

Over the past few weeks and months, the .NET and MAUI community has been running at full speed. Not only did this year’s [.NET Conf](https://www.dotnetconf.net/) take place, but MAUI is now receiving strong open-source support from [Syncfusion](https://devblogs.microsoft.com/dotnet/dotnet-maui-welcomes-syncfusion-open-source-contributions/), and [JetBrains Rider](https://www.jetbrains.com/rider/) has been made free for non-commercial projects. Still, the search for the right direction for [MyLife.NET](https://github.com/tscholze/dotnet-mylife) continues.

The .NET Conf Fireworks
-----------------------

Like clockwork, .NET bumps its version number every year—sometimes with bigger changes, sometimes with smaller ones for the community and developers. Kevin has already covered this thoroughly in [various articles](https://www.drwindows.de/news/net-conf-2024-microsoft-veroeffentlicht-net-9-mit-vielen-verbesserungen), so I’ll just say that I’m placing a lot of hope in this release. It’s not that I fully understand all the low-level changes in the framework (the well-known .NET developers James Montemagno and Frank Krueger provided great insights in [an episode of their podcast](https://www.mergeconflict.fm/437)), but it feels like there’s renewed energy and a clearer plan behind this open-source platform.

Many frameworks built on .NET held their own releases for .NET 9, which resulted in a satisfying cascade of updates across the still-growing .NET ecosystem.

### More Focus on Desktop?

One positive trend I noticed was a possible renewed focus on desktop applications, which I’ve missed in the past. There were demos and talks from partners building desktop apps with MAUI, and shout-outs to third-party frameworks like [Avalonia](https://avaloniaui.net/) for running on Windows PCs. The hoped-for GTK support for MAUI apps on Linux still hasn’t materialized for me, though.

### Lastly: Less Stiff, More Practical

For many, .NET development—and the framework’s image—still feels tied to rigid enterprise processes: consultants in suits and legacy software that hasn’t been touched since grandpa’s 640×480 monitors. The Microsoft team, led by folks like Maddy Montaquila, keeps bringing fresh energy in their presentations and keynotes to show that those days are over and that .NET, C#, and friends are “hip” again—whatever that may mean.

### The Long-Awaited MAUI Update

[As part of .NET 9](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9/overview), .NET MAUI received several long-awaited improvements. Beyond the usual emphasis on overall software quality, two new components were added to the standard MAUI toolkit: a new HybridWebView for apps that rely on HTML, CSS, and the like while still integrating closely with the system, and—specifically for Windows—a new TitleBar component that enables prettier, more productive title bars with built-in functionality.

For many developers, the wait for Xcode 16 and support for the current iOS and iPadOS versions is finally over. For those who’ve had trouble getting management to approve changes to minimum OS requirements, the race is now on to set the deployment target to at least iOS 12.2 in time.

These changes make .NET MAUI more robust, flexible, and easier to use, which improves the overall development experience. I’m also quietly hoping that interested developers will give .NET MAUI a chance before jumping to Flutter, Compose Multiplatform, or something web-based.

JetBrains Rider Now Free for Makers
-----------------------------------

This isn’t strictly inside the .NET ecosystem, but it’s a decision from JetBrains that will likely benefit the community: [JetBrains Rider](https://www.jetbrains.com/rider/) is now free for hobbyists and other non‑commercial users and projects. Anyone who was hoping for the integrated Hot Reload experience from WPF to also exist for MAUI will have to wait a bit longer. Given how quickly JetBrains ships new features, it wouldn’t be surprising to see these helpful tools arrive soon™.

Especially on macOS and Linux, there’s been a gap in capable IDEs since Microsoft sent Visual Studio for Mac to the afterlife. Yes, Visual Studio Code with the .NET Dev Tools extensions is—also for MAUI—almost a complete package, but it still doesn’t match Visual Studio or Rider. Here’s hoping 2025 moves the needle.

Visual Studio Code + .NET Keeps Getting Better — Still Not Quite There
----------------------------------------------------------------------

Microsoft is working hard to make Visual Studio Code a full-fledged development environment for .NET projects. In my experience, it’s getting better—especially for MAUI and XAML. I can honestly see VS Code becoming the tool of choice for C#/.NET work across platforms, whether on Windows or, in my case, macOS. Emphasis on “in the future,” though: right now it still doesn’t compare to Visual Studio or JetBrains Rider (which, to be fair, lacks a XAML preview for MAUI).

What’s Next?
------------

First up is updating and cleanup: bump NuGet packages, verify everything still works, and adopt improvements from the frameworks where it makes sense.

If you take a look at the current state, you’ll see it’s still very rudimentary. Clicking on the post tiles doesn’t do anything yet, and overall the app feels empty. I’ll bring it to life step by step and evaluate where certain user interactions make sense. If you have ideas, please share them as issues or in the comments!

I’ve also considered rebuilding this state using [MAUI Hybrid with Blazor](https://learn.microsoft.com/en-us/aspnet/core/blazor/hybrid/tutorials/maui?view=aspnetcore-9.0)—if only we had six-day weekends.

### The Calm of a Pet Project

Even though progress has stalled for various reasons, that doesn’t mean I’ve abandoned the project. I enjoy the freedom that comes with a “hobby project” status: no pressure to ship, just the option to sit down and work on feature X for MyLife.NET whenever I feel like it.

Want to Get Involved?
---------------------

If you’d like to contribute—whether to show a better or simpler approach, or to make your first contribution to a still-manageable .NET solution—you’re warmly invited to stop by GitHub, fork the [repository](https://github.com/tscholze/dotnet-mylife), open issues with problems or suggestions, or just browse around. We’re all here to learn and, more importantly, to have fun building software!