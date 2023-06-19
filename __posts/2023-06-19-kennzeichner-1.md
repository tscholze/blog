---
title: 'Devlog #1 Kennzeichner - Bing AI chat as a data source'
date: '2023-06-19'
abstract: 'Is it feasable to use Bing AI chat as a data source for public available information?'
---

I have often started new crafting projects. Some could be completed, others like our ["DuoBahn"-](https://www.drwindows.de/news/duobahn-das-entwicklertagebuch-teil-5-alles-ohne-google-wieder-auf-anfang)App remained unfinished due to external influences like wars. The beauty of such projects is that this is perfectly, okay! So let's start a new one!

## Hello, new tinkering

I don't want to reveal too much about our next app yet, just two facts to mention. The name of the app will be "[identifier](https://github.com/tscholze/kotlin-kmm-kennzeichner)" and we have to gather the data base for this ourselves. A stroke of luck for this is on paper that all the information needed for this would have to be usable under "Open Data" i.e. as "freely available data".

## This is the goal

The app Kennzeichner should enable the user to assign a license plate to a city or region on the basis of the area identifier. For a search for "A", "Augsburg" would appear, but also "Aalen", since the identifier "AA" is used for this. Metainformation about the respective regions in Germany should contribute to a certain added value. Which these are has not yet been determined. Furthermore, this app, with what appears at first glance to be a simple usage scenario, is also intended to sound out the experiments for possible extensions such as a voice-based search. Thus, it is difficult to determine the "final" goal of the app. Rather, they are steps in a certain direction, with which I want to learn a lot, but also celebrate quick successes without having to build weekend after weekend on a feature.

## KMM – Kotlin Multiplatform Mobile

Unusually for a Microsoft-related portal, this app is built on the [Kotlin Multiplatform Mobile](https://kotlinlang.org/lp/mobile/), or KMM, approach in the first stage. No Xamarin, no [Blazor Hybrid](https://learn.microsoft.com/en-us/aspnet/core/blazor/hybrid/?view=aspnetcore-7.0), no .NET [MAUI](https://learn.microsoft.com/de-de/dotnet/maui/what-is-maui?view=net-maui-7.0). There will be more explanation on the reasons in future articles as I get further into the subject of KMM. At the moment I can only say that the quality of .NET MAUI until the end of last year, also concerning the VS for Mac support, did not exactly suggest me to use this tech stack consisting of C#, XAML and the MAUI framework.

Whether MAUI might have been the better choice after all, we will probably know at the end of the journey with this tinkering project. A big plus point, which MAUI stands for, is the possibility of only having to write the app interface once in order to be able to use it on all platforms. One approach to this from the Kotlin ecosystem would be "Compose Multiplatform". For this I have privately [programmed an example](https://github.com/tscholze/kotlin-kmm-compose-sample). At a later point in the project, I will re-evaluate whether I want to work with platform-specific interfaces or with a split one - despite the possible disadvantages.

### Microsoft is still present

Still, Microsoft is broadly represented in the developer universe. Of course, the Android app will be made fit for the Surface Duo, if it still exists by then. Even without the Duo, products from Redmond can still be found. On the one hand [GitHub](https://github.com/tscholze/kotlin-kmm-kennzeichner) for source code and ticket management and possibly [Microsoft App Center](https://appcenter.ms/) for possible distribution of Android test versions. Whether the app will use [FluentUI from Microsoft](https://github.com/microsoft/fluentui-android) is largely up to how far Jetpack Compose and SwiftUI support has progressed by the time we get to that point per se.

## Bing AI as data basis

As mentioned above, the app needs a certain data base, which, according to my research, does not yet exist. I am looking for all currently used license plates, their region as well as their geographical location. In addition, optional meta information about the license plate region should be provided. In my eyes, this is a perfect hook to get involved with Bing AI Chat.

The beginnings of my conversation with the AI were very promising. Bing understood my concerns, asked specific follow-up questions, even offered to create an Excel spreadsheet from the data and send it to my email address on file. I was impressed and also excited - but then everything ended differently than expected.

Although Bing assured me that it would send me a message, it never arrived. To investigate this, I asked Bing what address it was sending the list to and how much memory it was using. This allowed me to rule out a problem with incorrect spam mapping or an email address that was not stored correctly.

During further attempts, my use of AI failed with different problems, which can nevertheless be summarized as "data export error". A direct output as CSV text in the conversation view was torpedoed, since here the texts were always shortened in the middle by means of "...", despite request to refrain from this.

On direct commands to email me the created file, Bing admitted to me that this was not yet possible. These restrictions were confirmed by [Microsoft expert Raphael Köllner on Twitter](https://twitter.com/Ra_Koellner/status/1635002907253952512?s=20). So why did AI offer it to me on the very first run?

The question remains whether Excel Copilot, which was in closed beta at the time of this article, could have solved this problem better.

### Bing AI chat prompt expert wanted

If you are familiar with AI chat requests and have managed to successfully resolve my issue, please share your prompt with the Dr. Windows community so we can learn together.

## What's next?

The tinkering project can also be run with demo data at the beginning. So this does not block the active development of the app. I will get more familiar with Kotlin and KMM in the coming weeks. The tinkering project can be found as often as [repository on GitHub](https://github.com/tscholze/kotlin-kmm-kennzeichner). I don't ask you to actively participate in the development yet, because I want to explore the framework and the possibilities of my idea first. Nothing is worse than your wasted lifetime. If you want to talk about KMM, Kotlin, Jetpack Compose and Co., feel free to join me on the [Discord Server of Kobweb](https://discord.com/invite/5NZ2GKV5Cs) (an essay on Compose for Web).