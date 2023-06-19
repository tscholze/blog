---
title: 'Devlog #3 Kennzeichner - Let us start with the shared core'
date: '2023-06-21'
abstract: 'One of the biggest advantages of KMM is the shared core between all consuming apps.'
---

Now we are finally getting started. We are starting with the development of the app. For the time being, this is divided into three PoCs - proofs of concepts. The first of these is the so-called shared core. This is the module that will be shared between the two apps in the future. The Android app will then be developed, and last but not least, iOS.

For all those who are still unfamiliar with this series of articles - you've missed it so far, but you can of course read up on it later:

* Developer's Diary: [Kennzeichner #1 using Bing AI chat as data basis](https://tscholze.github.io/blog/2023-06-19-kennzeichner-1.html)
* Developer diary #2: [Excel can do geography and doesn't like Berlin](hhttps://tscholze.github.io/blog/2023-06-19-kennzeichner-2.html)
* (current) Developer Diary #3: Shared Core - The Core of the Whole

## Explanation of terms

Terms and abbreviations are constantly dropped during the diary entries, which can quickly become a jumble in the learning process of the new technology stack. It is still difficult for me to explain all of this precisely, but in general KMM can be divided into three sub-areas. The article's comments are open for any corrections.

### Kotlin

... is a platform-independent and open source [programming language from JetBrains,](https://kotlinlang.org/) which is translated into bytecode for the JVM (Java Virtual Machine) similar to Java. Other languages for the JVM would be for example Groovy or Scala. It is de facto the language in which modern Android apps are developed, among other things. Kotlin to Java can be seen in the same relationship as Swift to Objective C.

### KMM

... stands for [Kotlin Mobile Multiplatform](https://kotlinlang.org/lp/mobile/) and means developing a common core of apps that run on different operating systems like Android or iOS. Throughout this series of articles, I will refer to the different components of a KMM project as ":shared" for the core, ":android" and ":ios" for the respective native apps.

In the structuring of KMM architectures, there is no hard limit to when it becomes platform-specific code and when it does not. To keep our entry into KMM simple, we make the cut at "data acquisition" and everything above that is developed per app. Here's how we lean on the second diagram in the chart below.

[![KMM Diagram](https://www.drwindows.de/news/wp-content/uploads/2024/04/Group-3.png)](https://www.drwindows.de/news/wp-content/uploads/2024/04/Group-3.png)

Through CMP (Compose Multiplatform), even the interface could be shared. This exciting approach has only just left the "experimental" phase and is in an alpha state at the time of this article. This would then make it possible to build websites using Compose. A feature I sorely miss in .NET MAUI.

### Ktor

... this [library](https://ktor.io/), maintained by JetBrains and developed in Kotlin, makes it possible to develop modern, lightweight and above all platform-independent http servers and clients. For the identifier, in addition to the client side of the framework, we also use its features around JSON deseralization and mapping.

### Gradle

... is the standard build management tooling behind modern Kotlin apps. Not only is it dependency management, but it can also handle a wide variety of tasks during the development of JVM-based apps. The obvious tasks would be building the actual app, embedding the split core, or even signing to ensure the file created is pristine. A key starting point to understand what dependencies or features are enabled on the part of Gradle is the respective module file [_build.gradle.kts_](https://github.com/tscholze/kotlin-kmm-kennzeichner/blob/main/app/shared/build.gradle.kts).

### Summary

Thus, our app identifier will be a KMM application written in Kotlin, which submits and processes web requests through Ktor. Gradle is used as the build chain tooling.

I will explain terms that will be used later in our tinkering in the following posts in this series.

## The core of the whole thing

The cornerstone is the module "shared", which contains the data acquisition, its processing and the state management. This in turn is subsequently integrated by the individual apps. This means that we only have to write the logic once. This also helps to ensure that both apps behave in the same way and have no differences in data management.

### Data retrieval

In the last post, we created the JSON file and put it on a GitHub Page so we can now grab it from within our repository (_[LicensePlateRepository.kt](https://github.com/tscholze/kotlin-kmm-kennzeichner/blob/main/app/shared/src/commonMain/kotlin/io/github/tscholze/kennzeichner/data/LicensePlateRepository.kt)_). A pure storage in Git and integration via GitHub link fails, because here no corresponding Http headers are set and the app could not use it.

Here, we don't have to reinvent the wheel and can fall back on proven frameworks from the Kotlin ecosystem. On the one hand, there is the "Ktor" framework, which takes care of the entire network communication up to the validation of the received data streams. On the other hand, the [coroutines of KotlinX](https://github.com/Kotlin/kotlinx.coroutines) (Kotlin eXtensions) also find use in the marker app.

[![](https://www.drwindows.de/news/wp-content/uploads/2024/04/carbon-4-1024x428.png)](https://www.drwindows.de/news/wp-content/uploads/2024/04/carbon-4.png)

As you can see, the program code is so far only designed for the happy path and still lacks any error handling in case of a server problem or a client-side incident, such as when the user is not connected to the Internet.

The list of dependencies will probably grow steadily as development progresses. Nevertheless, I try to reduce it to the minimum here and if really necessary, then fall back on well-known representatives of the respective libraries.

A good learning was that for multiplatform several http engines are needed, since none of them supports iOS and Android simultaneously, for example. I solved this by using an expect/actual implementation of the HttpClient factory. On iOS, the "Darwin" engine is used and Android uses "CIO".

### Data preparation

Since I deliberately kept the DTO ([Data-Transfer-Objects](https://en.wikipedia.org/wiki/Data_transfer_object)), i.e. the data that the server sends, dumb, a preparation step is interposed. In this so-called mapping from DTOs to DMs ([Domain Model](https://en.wikipedia.org/wiki/Business_object)), data is grouped, validated and also converted between types according to the intended use.

All of this is done with the idea of making it as convenient as possible for the apps to display the data, rather than adding extra work in terms of data hygiene. When developing for multiplatform apps that have native interface subprojects, it is important that Android and iOS developers feel comfortable and at home using the shared component. There will be extra sprints focused on this in the future.

[![](https://www.drwindows.de/news/wp-content/uploads/2024/04/carbon-5.png)](https://www.drwindows.de/news/wp-content/uploads/2024/04/carbon-5.png)

## Data State Management

In the first step this is implemented in a very rudimentary way. The repository in the shared module contains a list of already loaded and prepared data objects. Since the data does not really change, the first step is to check in a simple way if the list is empty. If yes, load data, otherwise the already existing data sets should be used. If in the future the API becomes more dynamic, this logic needs to be extended, for example, with a "last updated" hint.

One feature of the app is filtering loaded data. This also takes place within the shared repository. Here, in the future, we can expect to switch from simple lists to flows, data streams, to listen at various points for filtering, sorting, or other reasons why the indicators to be displayed change.

## Upcoming steps

The next step is to develop the first version, the proof of concept, of both native apps that will incorporate the shared core that has now been created. Once these two milestones are also taken off the marker, it's on to the polishing, the documentation and the nice little things that the app should have. These include, for example, Surface Duo support for Android or the integration of voice assistants in general, so that license plates can be found by voice alone.

If you have any open questions about contributing to the app, email me at [\[email protected\]](https://www.drwindows.de/cdn-cgi/l/email-protection#24504b464d4557644056534d4a404b53570a4041) or create an issue right now on [GitHub, where the complete source code of the app is](https://github.com/tscholze/kotlin-kmm-kennzeichner).

Let's continue to work together on this exciting tinkering project. Thank you for your continued interest in this nerdiness.