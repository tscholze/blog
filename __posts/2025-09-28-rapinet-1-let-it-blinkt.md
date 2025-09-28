---
title: '.NET on Pi: Let it Blinkt! with Copilot'
date: '2025-06-14'
---

![Raspberry Pi meets .NET: Make Blinkt! shine with Copilot](https://www.drwindows.de/news/wp-content/uploads/2028/06/DrW-cover-3.png)

The Raspberry Pi is no longer just a toy for tinkerers—it’s a serious platform for IoT projects. And thanks to .NET, development on it is not only powerful but also pleasantly convenient. With Copilot and the like, you can control LEDs or read sensors without having to dig deep into GPIO documentation. But is that still fun?

Let it Blinkt!
--------------

Today we’ll look at how to have some fun with blinking lights on a Raspberry Pi using .NET and the [Pimoroni Blinkt! HAT](https://shop.pimoroni.com/products/blinkt)—and let’s be honest: who doesn’t like blinking lights, especially when it doesn’t require studying schematics and pinouts?

Pimoroni’s Blinkt! HAT is an affordable and simple way to play with eight multicolor, dimmable LEDs. Behind the scenes, an APA102 acts as the controller.

.NET auf dem Raspberry Pi
-------------------------

### Installation

A few years ago, this was a pain—less fun than soldering chips onto boards by hand. Fortunately, that’s changed: these days you can get set up with a couple of copy‑and‑paste terminal commands. Microsoft also provides a [detailed guide](https://learn.microsoft.com/en-us/dotnet/iot/deployment) on their Learn platform.

### Raspberry Pi model

Personally, I’d use at least a Raspberry Pi 3 to run .NET apps. If you’re developing on the Pi itself—as we will—it’s better to use a Raspberry Pi 4 with at least 4 GB of RAM.

Local, yet remote development
----------------------------------

I’m old‑school and used to managing Raspberry Pis only over SSH—which isn’t strictly necessary anymore—so I decided to connect to the Pi from my Windows machine using Visual Studio Code. That way the source code lives on the Pi, but the editor runs on my more powerful system. This worked really well. Since you’re working in a fresh remote environment, you’ll need to (re)install the required VS Code extensions—but that’s basically just pressing a button in the editor UI.

Of course, with today’s Raspberry Pis you could also install VS Code directly on the device and attach a monitor, or mirror the display via Raspberry Pi Connect in the cloud—similar to VNC or RDP.

Development
---------------

### False friends along the way

Heads up: at first I fell for a “false friend,” the built‑in [.NET APA102 class](https://learn.microsoft.com/en-us/dotnet/api/iot.device.apa102.apa102?view=iot-dotnet-latest). Yes, it’s a full controller for the underlying chip, but we don’t talk to the APA102 directly—we talk to the Blinkt! HAT, which abstracts that away. So we can’t address it directly, and therefore we can’t use that built‑in .NET goodie here.

### Copilot agent, better than expected

I wanted this little project to be a playground for “vibe coding”—letting the AI, Copilot, do the heavy lifting. I initially assumed it would fall apart on such a niche topic—not at all. I was genuinely surprised by how much Copilot got right and working on the first try.

As a starting point, I showed it Pimoroni’s open‑source Python library for Blinkt!, and with a bit of reasoning and self‑correction it produced a first draft that actually worked.

Afterwards I had it comment everything in idiomatic C# so I’ll still know what’s going on in the future. Code that’s available to others deserves a bit of documentation, too.

### Trust is good; verification is better

In a few places Copilot tripped itself up or suggested the wrong actions. For example, the .NET program doesn’t need to run with sudo (root privileges), and some variable names were odd. To be fair, that was often due to my own strong preferences for how things should look and be structured—something Copilot learned over time, so later tasks matched my taste better.

### Source code is available

In the spirit of our tinkering, the [source code is available on GitHub](https://github.com/tscholze/dotnet-iot-raspberrypi-blinkt). Play with it, create new light shows, and have fun with software development. Amusingly, I also had Copilot generate the repository’s README. I’m not sure I actually like that popular yet very extroverted README style. What do you think?

Let the fun begin
----------------------

As mentioned, building the prototype was quick, and polishing it took minutes, not hours. Then the real fun could begin—making things light up! I’d forgotten how bright the Blinkt! actually is and quickly set the brightness to 20%. That’s enough to see what’s going on; I don’t need a searchlight (yet).

Here, GitHub Copilot came back into play. It generated several math‑based light patterns so I could see more than just red, green, and blue LEDs. Once again I realized how satisfying it is to make something from the digital world tangible and visual. It still triggers the same fascination in me as back then with [our Blinkt! on Windows 10 IoT Core](https://www.drwindows.de/news/windows-10-iot-core-perfekt-fuer-maker-und-hobbyisten).

Side note: a video experiment
----------------------------------

As you know, I really enjoy getting people excited about software development—especially when it’s something anyone can replicate. As I’ve often written on [Dr. Windows](https://www.drwindows.de/news/windows-anleitungen-faq/wir-basteln-uns-einen-podcast-ein-erfahrungsaustausch-vom-einsteiger-fuer-einsteiger), I like to experiment with different forms of content creation, like my “La‑La‑Laber doch!” podcast a while ago. This time I wanted to follow in the footsteps of developer evangelists and publish a video showing how to make something blink with Copilot and .NET in under five minutes.

For context: I have a speech impediment, and it takes a lot of courage—and processing of shame—to publish or even prepare content in a foreign language on the internet. Hearing myself stutter while editing is not exactly pleasant.

So a big thank‑you to the wonderful Dr. Windows community! I don’t have to explain diversity and inclusion to you—you embody it, no matter what some despots say. Thank you!