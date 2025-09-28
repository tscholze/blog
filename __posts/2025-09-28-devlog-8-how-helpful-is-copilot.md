---
title: 'Devlog #8 MyLife.NET - How helpful is GitHub Copilot?'
date: '2025-04-12'
---

The internet has a new buzzword: [Vibe Coding](https://de.wikipedia.org/wiki/Vibe_Coding). In short, you no longer write the code yourself—you let AI do the work. Whether that really works and what else happened, you’ll find out in this new [MyLife.NET](https://github.com/tscholze/dotnet-mylife) developer diary.


What is Vibe Coding?
--------------------

The term “Vibe Coding” describes a method where developers use AI tools like Copilot to build software by giving the AI a list of natural‑language instructions. The AI then generates the code. The focus is on speed and creativity without having to write code yourself. It’s particularly suited to quick prototypes and small projects. Some of the biggest products in this space are the [Cursor](https://www.cursor.com/) editor and [GitHub Copilot](https://github.com/features/copilot), with the latter now offering far more than just code generation.

The term isn’t exclusively positive. It’s also used pejoratively because you might lack control or understanding of the “self‑developed” software and thus be more vulnerable to security issues.

What is GitHub Copilot Edits?
-----------------------------

GitHub Copilot is an AI‑powered assistant that helps developers write code faster and more efficiently. Copilot provides context‑aware suggestions, ranging from small line edits to generating entire functions.

GitHub Copilot Edits is an extension aimed at editing and refactoring entire projects. With Edits, developers can make changes across multiple files by describing a list of requirements in natural language. Roughly speaking, it’s like writing a traditional requirements or specification document. The tool then proposes targeted file‑based code changes that you can review and adjust directly in the editor.

On request, GitHub Copilot can also explain why certain changes are proposed. That helps me dig a little deeper—assuming you trust the explanations.

My experience
-----------------

First things first: I’m still quite skeptical about “vibe coding” and AI‑assisted development, so consider this part of the diary subjective. I also used the free Copilot plan, which in my experience mainly differs in quota—not quality—compared to the paid tiers.

[![](https://www.drwindows.de/news/wp-content/uploads/2026/04/Bildschirmfoto-2025-04-04-um-11.46.17-1024x864.png)](https://www.drwindows.de/news/wp-content/uploads/2026/04/Bildschirmfoto-2025-04-04-um-11.46.17.png)

### Starting setup

As a base setup I use the stable build of Visual Studio Code with Microsoft’s official .NET, C#, and MAUI extensions. For AI I use the GitHub‑published Copilot and Copilot Chat extensions. All of this runs on a Mac mini with an M4 SoC. As usual, MyLife.NET is a solution with sub‑projects per feature—for example, Core for shared functionality and MAUI for the iOS and Android app.

This was my first time working directly with Copilot, so I eased into prompt writing during the experiment—which may have affected the overall quality of this evaluation.

### Confusion was confused

I’ll be honest—I hope it’s all my fault! If what I describe below is what other developers are seeing too, then good luck to us all.

Visual Studio Code often seemed unsure whom to trust: its own code completion or the AI’s suggestions. Once the editor or I picked one, syntax coloring turned into more of a rainbow than any unicorn has ever seen. I’m a very visual developer and rely heavily on syntax highlighting—unexpected changes confuse me and distract me more than some compiler errors.

### Careless mistakes galore

I also noticed the AI sometimes made very easily avoidable mistakes. For example, it added C#‑style comments inside XAML files (which are, roughly speaking, XML), or it gave XAML elements parameters they simply don’t have—at least not in the XAML dialect that MAUI uses.

### Would tests help?

The assistant often left code that no longer compiled or changed behavior unexpectedly. Would a high, automatically verified test coverage be a good aid here, enabling the AI to correct the errors it introduces?

### Help beyond code as well

What I really came to appreciate were tips outside of code changes—for example, how to enable the genuinely useful “Hot Reload” feature in Visual Studio, or how to start an Android (alas) emulator from the command line. These were the little heroes for me, nudging me to search the web less and simply ask the AI.

### Good prompts are genuinely hard

Many of these problems might be mitigated with better prompts. I may also have expected too much “context knowledge”—like recognizing that it’s a MAUI project. Writing longer prompts is still hard for me; perhaps the Voice extension could help by letting me dictate them.

### Don’t lose trust

I’m a 30‑something developer from Bavaria; I’m sometimes hard to convince to trust something new—especially when it proves a bit flaky. Still, GitHub Copilot has shown me often enough that I need to broaden my perspective. Much of the output and assistance was demonstrably helpful, and it always takes two to make a mistake—the AI and the human who asks it.

Once I have credits again, I’ll keep using Copilot and gather more experience. Whether I want to spend money right now just to make more requests is something I still need to think about.

What else happened?
-----------------------

### Sometimes you just have to read the manual

I had to set up my .NET environment on a new machine and dreaded it: something always broke, paths were wrong, and so on—just thinking about it gave me chills. This time I did something we should all do more often: follow the instructions step by step instead of thinking “ah, I’ll do it my way.”

The end result: everything worked much faster and without a single error.

### New features?

As mentioned, I mostly used Copilot to improve the current codebase and add missing comments and the like. The only new feature during my evaluation was the start of the “Open Source” view in the .NET MAUI sub‑project, which is slowly taking shape.

### Grouped list vs. vertical layout

For the Open Source view I wanted to use a grouped list like in the Blazor project. That doesn’t seem feasible with the current data structure. It may need to be just a string for the section name and a list of items for the entries.

So in the near future I’ll switch this from a grouped list to a vertical stack layout. It’ll be interesting to see whether it loads dynamically or the view is built in one go with lots of images and text, which could lead to a sluggish UI—we’ll see!

### Xcode 16 / iOS 18 ready

As is often the case with cross‑platform projects—be it .NET MAUI, React Native, or Flutter—when a new Xcode drops, things can break, especially if you update it by accident. This time, however, everything went smoothly, and our app is ready for Xcode 16 and iOS 18.4—no tears shed.

What’s next?
-------------------

### Focus on MAUI

First, I need to carve out more time—those weekend coding sessions are getting rare as life moves on. Nevertheless, my focus is on the .NET MAUI app to bring it to feature parity with the Blazor app.

For everyone asking about an app release: No, that’s unlikely in 2025. It’s not a focus right now, and I don’t see the value at the moment.

### The web won’t be forgotten

Next I absolutely need to work on the design of that component. My CSS—which I’ve been lugging around for two decades—unsurprisingly feels outdated. I still don’t want to use anything generic though—maybe AI can help here?

Want to get involved?
--------------------------

If you’d like to contribute—whether to show me a better or simpler way, or to make your first contribution to a still‑manageable .NET solution—you’re warmly invited to stop by [GitHub](https://github.com/tscholze/dotnet-mylife), fork the repository, open issues with problems or suggestions, or just browse around. We’re all here to learn and, more importantly, to have fun building software!