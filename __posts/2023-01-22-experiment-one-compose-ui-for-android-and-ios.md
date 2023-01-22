---
title: 'Experiment: One Compose UI for Android and iOS'
abstract: 'Would be a shared ui between iOs, Android and Desktop a great idea? Let's test!'
date: '2023-01-22'
---

I am not an Android guy, I do not like all the hoops Android developer has to go with all the variations of devices out there. 

Yet, there is always a "but" and the "but" in this case is productivity and sharing of available work force between departments. In the case of a shared UI based on Jetpack Compose it would be the freely moving developers from the Android and iOS side.


## New playground!

To get myself into all of these related topics such as Kotlin, Kotlin Mobile Multiplatform, Jetpack Compose, Gradle I started a sample project in which I can play around.

The sample simply called *CMP Sample* which is in my speak the short form of **C**ompose **M**ulti **P**latform is a playground to check which hoops and loops I have to go. And boy, there are plenty of them - especially for a beginner!

![Version 0.0.1](https://github.com/tscholze/kotlin-kmm-compose-sample/blob/main/docs/v001-min.png?raw=true "Version 0.0.1")

## What it's not

This is no production ready source. This will be never released as an app store app. This is - as the name suggests - a playground of an absolutely beginner! I really hope my skillset will grow along my journey, but for now, it is better to be motivated by this project to get into shared Compose ui, than take it as a "it has to be that way".

Nevertheless, the tech stack it self it far away from beeing ready for production, too.

## (Planned) features

### Setup
- [x] Setup shared ui `:shared:commonMain` to contain Jetpack Compose ui
- [x] Setup `:shared:androidMain` to containerizes and use `:shared:commonMain` ui
- [x] Setup `:shared:iOSMain` to containerizes and use `:shared:commonMain` ui
- [x] Setup a new `:desktopApp` that must use shared ui
- [x] Strip down `:androidApp` to use shared ui
- [x] Strip down `:iOSApp` app to use shared ui
- [x] Get rid of `:iOSApp` build log flodding

### UI
- [ ] Create a fake but real world example of an app ui
- [ ] Evaluate how the translation from Android JPC controls like `BottomBar` works in iOS
- [ ] Evaluate how it would be possible to create "custom" ui for each platform but use it in `:shared:commonMain`

### Shared functionality
- [ ] Evaluate how to use features like pushes
- [ ] Evaluate best practices for logging and crash statistics

### Instruments (Performance check)
- [ ] Check how's the memory consumption
- [ ] Check if there is no memory leakage

### and a lot more
I think the features will grow and shrink and grow again as I walk my way through this new experiment of developing mobile applications.

## Special thanks to
to be clear, I would not get this "far" without the help of Kotlin, KMM and Compose giants like [David Herman](https://github.com/bitspittle) and [Adrian Witaszak](https://github.com/charlee-dev). It is awesome to read their ideas, to study their source code to get a better understanding in a rapidly changing technology.

## Get in touch
If you wanna talk about Kotlin, KMM, Shared Compose UI, Jetpack Compose pr Jetpack Compose for Web, I would highly recommend to join David's tiny but [awesome Discord server](https://discord.com/invite/5NZ2GKV5Cs) for his awesome CfW-enhancing web framework called [Kobweb](https://kobweb.varabyte.com). I never meet such brilliant folks that are not too "good" to help beginners.