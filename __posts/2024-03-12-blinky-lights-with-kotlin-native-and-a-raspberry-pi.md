---
title: 'Blinky lights with Kotlin Native and a Raspberry Pi'
abstract: 'Using Kotlin Native to let a Pimoroni Blinkt! light up on a Pi via a browser'
date: '2024-03-12'
---

The days are getting shorter and more uncomfortable. This not only means that Christmas is approaching, but that we at Dr. Windows are tinkering again! This time with a Raspberry Pi, glowing LEDs of the [Pimoroni Blinkt! HATs](https://shop.pimoroni.com/products/blinkt) and [Kotlin Native](https://kotlinlang.org/docs/native-overview.html).

The current project can be seen as an imaginary successor to our [Windows 10 IoT Core Apps](https://www.drwindows.de/news/windows-10-iot-core-perfekt-fuer-maker-und-hobbyisten), which we developed several years ago. And all this, even though I don't have the faintest idea.

## Why no more Microsoft platform?

A very legitimate question. Especially since we are on a news site that is rather close to Redmond. To be honest, I would like to continue developing for Windows IoT Core if it still existed. [Microsoft has let it die on the vine](https://www.drwindows.de/news/microsoft-und-co-haben-keine-lust-mehr-auf-hardwarebastler), and in the state it's currently in, I simply can't advise anyone to engage with this operating system or ecosystem in their spare time.

## Why Kotlin Native?


Now that Microsoft has not only discontinued the operating system for the Raspberry Pis, but also recently discontinued Visual Studio for Mac (https://www.drwindows.de/news/visual-studio-microsoft-stellt-den-ableger-fuer-macos-ein), .NET and C# are no longer a path I want to take in the hobby environment. I love the freedom to choose my host operating system and possibly my development environment so that I don't feel constrained. Kotlin and the IDEs from JetBrains run on the Mac as well as on Linux and Windows machines and are also available in free versions for the use case we have in mind.

Furthermore, for me personally, Kotlin is the language of choice when it comes to platform independence and versatility. Be it app and desktop development with [Compose Multiplatform](https://www.jetbrains.com/de-de/lp/compose-multiplatform/), server things with [Ktor](https://ktor.io/) or now also Raspberry Pi with [Kotlin Native](https://kotlinlang.org/docs/native-overview.html).

This naturally leads to problems of its own, especially if you want to learn this language. Because Kotlin is not the same as Kotlin as far as the range of functions is concerned. This is all the more true if, like us, you want to go the "native" route in this tinkering. There is no JVM, which otherwise conveniently abstracts away the peculiarities of the operating system.

The plus point is that you can run Kotlin code on platforms that do not have a JVM, such as iOS, or on those that are simply too weak for it, such as older Raspberry Pis and other single-board computers.

What distinguishes Kotlin Native from standard Kotlin is that, like a C# program, a single executable file is created, which can be viewed like a _\*.exe_ under Windows. Of course, like many things in the Kotlin universe, this has a "k" prefix and is called \*.kexe - ideal for cookie time in winter.

## What are we making?


We are building a program with which we can control the pixels of the [Pimoroni Blinkt! HAT](https://shop.pimoroni.com/products/blinkt?variant=22408658695)s of a Raspberry Pi as an example for later use. Not only hard-wired via source code, but also via a web interface. Instead of just eight pixels, this could also be used to implement Christmas tree lighting. APA102-based lights are also available by the meter.

In technical terms, we use GPIO to control voltage changes on eight AP102 pixels, each of which controls a red, green and blue LED. For the _curl_\-API or the web interface, we use a web server embedded in the program based on Ktor, which, like Kotlin itself, also comes from JetBrains.

[![We make blinking lights with the Raspberry Pi and Kotlin Native](https://www.drwindows.de/news/wp-content/uploads/2023/11/flow.png)](https://www.drwindows.de/news/wp-content/uploads/2023/11/flow.png)

Unlike back then with our Windows 10 IoT Core apps, this time we have to take care of running and starting the app from our development environment ourselves. This is easier than we thought, as we either have a built-in shell on the host computer if we are using Mac or Linux, or we can retrofit it under Windows with the WSL.

Thus, a small bash script takes care of building, uploading to the Pi and starting the program. With this simple approach, we lose the ability to stop and examine - i.e. debug - the program running on the mini-computer during execution.

The source code and an explanatory readme file are stored in the corresponding [GitHub repository](https://github.com/tscholze/kotlin-kpi-native-blinkt). You can see what it looks like in the end in the video at the end of this article.

## Dependencies used

We are not reinventing the wheel, which is why we are relying on the [Framework KtGpio](https://github.com/ktgpio/ktgpio/) for this tinkering, which in turn is based on the C libraries that we have to link in the classic way when building the app. It would simply be too much to start from scratch with this type of development. It's the same with [Ktor](https://ktor.io/), which is our library for the web server.

It's nice when you want to understand and grasp things from the ground up. But I don't think it's worth investing months in order to achieve the actual goal with this fiddling around.

If you also want to tinker with the Pi but don't want to do without the JVM, we recommend the [Library Pi4J](https://pi4j.com/) with the Kotlin DSL plugin.

## Project structure & important sections


As our tinkering is a small program, the project structure is also very simple. In addition to the Main.kt to start the program, there are the self-describing packages "_APA102_" and "_Server_". Communication between the server and the eightfold APA102 controller takes place via a _[MutableStateFlow](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-mutable-state-flow/)_, which transmits the "commands" one-sided, so to speak.

### APA102.kt

This is the manager ([GitHub file](https://github.com/tscholze/kotlin-kpi-native-blinkt/blob/main/src/nativeMain/kotlin/io/github/tscholze/kblinkt/apa102/APA102.kt)), which lights up the lights using GPIO connections. The corresponding pin numbers are defined at the bottom of the Kotlin class as well as the number of pulses required to trigger certain reactions of the chip. This is common practice when you are working "low-level". There are also the following essential functions for controlling the APA102:

#### writeLedValues()

We can change our Kotlin objects as we like, but ultimately we also have to communicate these to the respective APA102 chip. This is done with some bitshifting and the writeByte function for each of the eight available APA102 chips.

The clock, i.e. the clock pin, must first be locked or unlocked so that the chips accept the desired configuration.

#### SetClockState()

By means of an exact number of pulses, we make the chips listen to changes. Depending on the component, this is a different number, in our example it is 36 to lock the clock and 32 to unlock it again. If my understanding is wrong, please correct me in the comments.

#### writeByte(input: Byte)

After the chips listen for changes, the actual information can be written as bytes. I did not invent this logic, which is complex for me as a high-level language developer, but took the official [Pimoroni Python library](https://github.com/pimoroni/blinkt/blob/master/library/blinkt.py#L49) as a strong inspiration.

### Server.kt

In the actual sense, this is not an independent class ([GitHub file](https://github.com/tscholze/kotlin-kpi-native-blinkt/blob/main/src/nativeMain/kotlin/io/github/tscholze/kblinkt/server/Server.kt)), but a function that returns a corresponding interface of a created web server. It can be that simple these days.

#### runServer(actions: MutableSharedFlow<Command>)

The function's eponymous task is to start an embedded web server. This listens to port 8080 and offers a _GET_ and several _POST_ requests.

An example of a _POST_\ request that contains user-defined content is the one for the "morse" functionality. Here, the "_call.receiveText()_" method provided by Ktor evaluates the payload provided by the sender, which in our case contains the word to be morse.

#### val template

Since we have no access to the resources in Kotlin Native and the data input and output is very complex, I decided to store the web interface as a Kotlin string. This is returned to the requesting browser in _GET_ on "/".

So that I can edit the html properly, the template is available as a simple _\*.html_ page in the file "_index-raw-template.html_", in which the editor can use its full power in terms of syntax highlighting and code completion.

### Conclusion

Getting started with Kotlin Native was very bumpy because it is not JVM-based and, as a beginner, it was often not clear to me during my research whether the function I was looking for was only available in the JVM, in JavaScript or as a native variant.

Nevertheless, I enjoyed it and it showed me that you don't always have to use Python if you want to have fun with a Raspberry Pi - even if it's getting on in years.