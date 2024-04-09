---
title: 'Using Kotlin Native to read BMP280 and TCS3472 sensor data'
abstract: 'Using Kotlin Native on a Raspberry Pi to read temperature, pressure and ambient light data from a BMP280 and TCS3472 sensor'
date: '2024-04-09'
---

A Raspberry Pi, a few sensors from Pimoroni, a bit of Kotlin/native source code and the latest addition to our "Dr. Windows tinkers" series is ready: [KPi.Enviro](https://github.com/tscholze/kotlin-kpi-native-enviro). But it wasn't an easy walk for me until then. For unexpected reasons, the BMP280 and TCS3472 sensors proved to be uncooperative for me as a newcomer.

## Explanation: Difference Kotlin/Native to Kotlin

[Kotlin/Native](https://kotlinlang.org/docs/native-overview.html) (K/N)is a variant of the well-known Kotlin, which aims to compile source code into native binary files. These files can be executed directly without a (Java) virtual machine (JVM) - like a \*.exe.
In view of this, K/N is suitable for devices on which no VM is present, such as embedded devices or even iOS and iPadOS. The linchpin of K/N is the simple, but mandatory, embedding of C bindings. In our example, these bindings enable access to the GPIO pins, for example.
What I understand by Kotlin so far is [Kotlin/JVM](https://kotlinlang.org/docs/jvm-get-started.html). This is where the largest ecosystem of Kotlin, Java, Groovy and Co. come together and, with a wide range of libraries and frameworks, ensure that there is nothing that cannot be solved with Kotlin. Server and Android programming, Minecraft and thousands of other products rely on the JVM variant.

## Longing for Windows 10 IoT Core

In the last part of our ["Kotlin/Native" series on the topic of "Blink HAT"](https://www.drwindows.de/news/wir-basteln-blinkende-lichter-mit-dem-raspberry-pi-und-kotlin-native), I explained why I moved away from the usual [Windows 10 IoT Core](https://learn.microsoft.com/de-de/windows/iot/product-family/windows-iot) and C#. During the development of [KPi.Enviro](https://github.com/tscholze/kotlin-kpi-native-enviro), I often thought back to the golden age when Microsoft still catered to niches such as office hobbyists and the like. Because one thing should be said here: in the .NET environment, it's much easier for beginners like me to get started with low-level sensor readout, as .NET comes with more preconfigured features. For K/N, I continue to rely on the [ktgpio](https://github.com/ktgpio/ktgpio) library, which unfortunately no longer seems to be maintained.

What stopped me in the end was the fact that there is no Windows 11 IoT and that the development of the tooling for the IoT version of Windows 10 was apparently simply stopped in the middle of the process. Yes, I mourn the Windows 10 IoT era, but unfortunately I won't be able to revive it.

## What are we making this time?

After lighting up a few LEDs last time, this time we're going a little deeper into the subject. We want to read out sensors! Specifically, this involves temperature, pressure and altitude values from a BMP280 ([Bosch data sheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bmp280-ds001.pdf)) and some ambient color detection using a TCS3472 chip ([data sheet](https://cdn-shop.adafruit.com/datasheets/TCS34725.pdf)).

Everything together is conveniently placed on a meanwhile discontinued Pimoroni pHAT, which in turn is plugged into an aging but still fully adequate Raspberry Pi 3B with a 64-bit Raspbian operating system.

[![Diagram](https://www.drwindows.de/news/wp-content/uploads/2025/03/scheme-643x365.png)](https://www.drwindows.de/news/wp-content/uploads/2025/03/scheme-e1711045112929.png)

At the end, the values read out should be readable from anywhere in the local network using a rudimentary website.

## Lessons learned from Blinkt!

None of this would make sense if we didn't learn from our mistakes. In the case of [KPi.Enviro](https://github.com/tscholze/kotlin-kpi-native-enviro), from possible improvements to the predecessor [KPi.Blinkt!](https://github.com/tscholze/kotlin-kpi-native-blinkt) learned in retrospect.

### Raspberry Pi operating system

Sounds obvious in retrospect, but choosing the architecture of the Raspberry Pi's operating system was a hurdle recently. Since my host system is 64-bit ARM and [Visual Studio Code Remote-Host-Extentions](https://code.visualstudio.com/docs/remote/ssh) can only access a 64-bit system, the choice fell on a 64-bit Linux, although a Raspberry Pi 3B with one gigabyte of RAM would feel comfortable under 32-bit.

### Simpler software architecture

In terms of software architecture, the connection between the website and sensors was in need of immense improvement and, above all, simplification. That's why I decided not to use the "Emit & subscribe" pattern for this weekend project. In other words, the app does not use the [SharedFlows, Emitters and Collectors](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-shared-flow/) known from Compose. This is not a development pattern for a productive app. For learning the basics, which do not concern reactive programming as I am aiming for, it can be shirt-sleeved. Anyone who can contribute knowledge in this regard, please accept my goodwill.

### Exit without regrets

In addition to this, the point of gracefully shutting down the chip controller and the web server was also something I took away from Blinkt! Takeaway. Low-level GPIO pins do not like to be simply switched off and on again. Old connections could still be active and therefore fresh connections caused by a restart of the app could be rejected by the system.

## Project structure & important sections

The app is separated into four parts, which all come together under the umbrella of Enviro.kt. Bmp280.kt, Tcs3472.kt and LEDs.kt take care of communication with the board components, while Server.kt is responsible for the web interface and the content connection to the aforementioned code sections.

### LEDs.kt

This is by far the [simplest area of the app](https://github.com/tscholze/kotlin-kpi-native-enviro/blob/main/src/nativeMain/kotlin/io/github/tscholze/kenviro/led/LEDs.kt) On the Enviro pHAT, both LEDs can be controlled via the same GPIO pin 4. Unlike with Blinkt! they cannot be colored or dimmed. Everything nice and straight forward.

### BMP280.kt

[This Kotlin class](https://github.com/tscholze/kotlin-kpi-native-enviro/tree/main/src/nativeMain/kotlin/io/github/tscholze/kenviro/bmp280) measures the information temperature, air pressure and altitude above zero from the chip controlled via I2c, depending on the previous value.

As can be seen from the Bosch data sheet, data is required to calibrate the temperature sensor, without which the calculation would run off the rails.

<script src="https://gist.github.com/tscholze/380bee7a1266c632151a5fdce35baf56.js"></script>

The actual calculation can then take place using the C algorithm from the data sheet. 

### TCS3472.kt

This light sensor communicating via I2c puzzled me, but the implementation of the red, green, blue and clear values recorded by the module was surprisingly simple. The challenge in this calculation lay in the correct bit masking of the memory registers with logical links. [The "driver" class](https://github.com/tscholze/kotlin-kpi-native-enviro/blob/main/src/nativeMain/kotlin/io/github/tscholze/kenviro/tcs3472/TCS3472.kt) uses the simplest method and, for reasons of maintainability, dispenses with further adjustment screws such as gain or sampling.

I cannot make a statement about the usefulness of mainly the clear value. In direct flashlight light, the largest possible value of 2 byte data types is given as 65,535. A value of 100 is still bright enough for a person to recognize objects without any problems.

### Server.kt

As mentioned at the beginning, everything comes together here for the sake of simplicity. This enables simple embedding of captured sensor data in the html page template. Since native file accesses are not as easy to implement under Kotlin as under Kotlin within a JVM, the basic web page structure is also integrated into the program as a simple string in Kpi.Enviro. Placeholders are located in the appropriate places, which are exchanged with the actual information using simple "replace" methods before the text is sent to the browser.

## Things I learned this time

Just like last time, I learned a lot while tinkering with the Enviro pHAT. For this reason alone, weekend tinkering like this is worth the time for me.

### Take nothing for granted!

The development under Kotlin/Native has shown me one thing: Don't take anything for granted that you've always been used to having! File accesses, _String.format()_ or other little helpers, which seem obligatory to every high-level language developer, are looked for in vain in this form under Kotlin Native - logically because there is no JVM, but C bindings, which in turn is the strength of this Kotlin style.

On the other hand, embedded and low-level developers feel at home here and may be able to say goodbye to their beloved C or Rust.

### Learn the size of your data types

An integer is bigger than a word is bigger than a byte and we don't even need to talk about short, unsigned bytes and their arrays. While programming KPi.Enviro, I became more sensitive again when it comes to dealing with data types. In modern apps, integers, doubles, strings and their lists are ubiquitous and you often don't think about whether you really need a 64-bit accurate data representation.
When addressing sensors, however, it makes a huge difference. If one, two or four bytes are queried, only fragments of a correct value may remain. Knowing how and when certain types produce a bit overflow, i.e. when the number stored in them becomes too large, does no harm either. Especially if this is a desired behavior on the part of the component.

### I miss a vibrant community!

I often felt alone - metaphorically speaking. Kotlin Native in itself is already a niche, but it is experiencing a certain influx due to the increasingly well-known Kotlin Mobile Multiplatform and iOS devices. However, if you move away from these architectures towards Rapsberry Pi and the like, things get bleak. If you then want to address other devices via GPIO pins, even the darkest night is even brighter. However, this could also show me that I need to change my way of thinking when it comes to Kotlin Native. Away from the big JVM ecosystem back to C bindings and that you simply have to take many things into your own hands without the magic of external libraries.

## Conclusion

Was it worth it? Definitely! Will I go on and talk to other Raspberry Pi HATs? But so what! Do I feel comfortable? Nope.