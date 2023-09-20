---
title: 'Devlog #4 Kennzeichner - Lets go mobile!'
abstract: 'Lets create an Android app that consumes the shared core'
date: '2023-09-20'
---

After we have laid the foundation for the actual mobile applications in the last articles, we will now start with them! We'll start with Android, since we can stay in the same ecosystem of Kotlin and co. for the time being.

For all those who are still unfamiliar with this series of articles - you've missed it so far, but you can of course read up on it later:

* Developer's Diary: [Using Bing AI chat as data basis](https://tscholze.github.io/blog/2023-06-19-kennzeichner-1.html)
* Developer's Diary #2: [Excel can do geography and doesn't like Berlin](https://tscholze.github.io/blog/2023-06-19-kennzeichner-2.html)
* Developer's Diary #3: [Shared Core - The Core of the Whole](https://tscholze.github.io/blog/2023-06-21-kennzeichner-3.html)
* (current) Developer's Diary #4: Lets go mobile!

## Why start with Android? 

I chose Android over iOS for the identifier ([GitHub](https://github.com/tscholze/kotlin-kmm-kennzeichner/tree/main)) because I needed something to test the shared module during development - unit tests and co follow. It's more convenient on Android since I don't have to switch the development environment from Android Studio to Xcode. And also faster, since there is no need to build a separate iOS framework, so it was easy for me to choose the "first" app platform. 

## Technology selection 

To say the least, my knowledge towards Android app development is less than compared to iOS. Thus, technologies used may not be selected correctly or used incorrectly. So if something is omnipresent, please feel free to leave an issue on GitHub. 

### Jetpack Compose 

To develop the interface, [Jetpack Compose](https://developer.android.com/jetpack/compose?gclsrc=aw.ds&gclid=CjwKCAjwp6CkBhB_EiwAlQVyxToi4ukbYgeVJC975bDFJGmBwYY5yuPOOKFWq4wz-6XKk-6EZxlJnBoCcm0QAvD_BwE) is used, which, like SwiftUI used later in the project, allows a declarative style in the description of the respective interfaces. In simple terms, this means defining views in the app using the same programming language in which the programs are written. In the past, this was often divided, so Android had Java and XML and iOS had Swift or ObjectiveC and the proprietary interface builder to give the logic a look. 

Another reason for Jetpack Compose is that for this, the perceived learning curve is more motivating than the old way of XML files, Activities, Fragments and Adapters. 

Of course, an ulterior motive is also that in this project in the future I would like to try to be able to share the interface developed in Jetpack Compose as well as the data logic from the Shared module between both apps.  

Inevitable for modern apps is a light, but also a dark mode, which adapts to the system. Thanks to the possibility of working with so-called themes - i.e. descriptions of colors, fonts and shapes - the effort for this is low.

Furthermore, Compose should not only be seen as a UI framework, but also as a kind of state-flow engine. However, I haven't come to this yet, because you should know the foundation before you put the antenna on the roof - in my humble opinion. 

### Koin as Dependency Injection 

Nowadays, Dependency Injection, DI, is an indispensable part of modern software development. [Koin](https://insert-koin.io/) is the de facto standard for Kotlin-based Android projects here. 

[![](https://www.drwindows.de/news/wp-content/uploads/2024/06/Snap-4.png)](https://www.drwindows.de/news/wp-content/uploads/2024/06/Snap-4.png)

DI makes it possible to access important helpers like services, repositories and co. without having to pass tedious, cascading variables. For those interested, "dependency inversion" is the buzzword and the [related article from Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#dependency-inversion) is recommended reading.  

In Microsoft's .NET includes this functionality itself and does not require external libraries for DI.  

### Material Design 2 

For my first attempt at designing an Android app, I used the dependencies on components from the Material Design 2 ecosystem that were stored in the project template at the time.  I was surprised that this did not make use of the Material Design 3 components, which are soon to be a year old. One reason for this could be that not all components are available yet, despite the age of MD3. Android connoisseurs are welcome to leave a comment.   

A migration to the now newest variant design system, [Material You](https://material.io/blog/announcing-material-you) (Material Design 3), is being considered, but not of high priority on my part.  

[![](https://www.drwindows.de/news/wp-content/uploads/2024/06/Frame-8.png)](https://www.drwindows.de/news/wp-content/uploads/2024/06/Frame-8.png)

## Development 

For any Android app developers among you, the current state of the app will be a task of a few hours, yet I'm very pleased with how it turned out for me as a newbie. Elementary for me in such tinkering and learning projects as the identifier is that the source code is simple, purposeful and before for me as a learner understandable.

Of course, this also means that in certain points you could have written things shorter or more concisely, or commented on things less expansively. But as mentioned before, I'm an egoist as a nerd, and if something in a tinkering project offers an added value for me, I prioritize this higher than the ultimate syntax-sugar. In the following, I would like to focus on a few, but key places in the source code of the Android app. 

### RegionsScreen.kt 

This screen or page from the identifier depends on two status values. If the app is still in the process of loading data, a loading animation is displayed, if this is completed, the status changes and the actual content of the view is displayed. In this view all available regions are shown in a list, it offers a search and filtering and allows to jump to the detail page of a region. 

Here I put a lot of emphasis on the reuse of certain interface components, which are also used in other places in the app. An example is the composable \`_RegionInformation(region: Region)_\`, which is not only displayed in a list entry, but is also used in the detail and map view. 

[![](https://www.drwindows.de/news/wp-content/uploads/2024/06/Snap-2-643x516.png)](https://www.drwindows.de/news/wp-content/uploads/2024/06/Snap-2.png)

### MapScreen.kt 

The marker app for Android is, of course, heavily based on Google Maps. It was exciting to see how the system or Google Maps would cope if you wanted to display several hundred markers, also called pins, at a glance. Of course, this is no longer a problem nowadays - in my early days, this would have been an impossibility, Windows Mobile 5 says hello. 

What I haven't managed to do yet is edit what the pin looks like. Right now the default marker is used with the accent color of the app. I would have liked to have had some sort of pseudo location sign. We'll see if I can figure that out during the life of the project. 

It should be noted that unlike Apple Maps on iOS, for example, to use Google Maps on Android, an API key must be created and it has a maximum number of requests per time period. With the identifier, we will probably never bump into this.  

### No Surface Duo support 

I was really looking forward to this and now I have really, once again, been harshly disappointed by Microsoft. With the Surface Duo "vegging out" it doesn't even make sense for me as a hobbyist to include the SDK anymore. Personally, I really believed in it and have already taken the first steps, but until Microsoft gives a sign of life or a statement of intent here, this feature is no longer planned for.  

## Upcoming steps 

The next step will be for the iOS app to see the light of day. This will certainly reveal a number of weaknesses in the shared API, which will need to be eliminated before further functionality is added to the core. Finally, of course, the documentation should also be considered. 

What functionality would you like to see in the identifier? If you want to start with Android app development as well, how can we take away your fear of this topic? Let me know, either in the comments or email me at [email protected](https://www.drwindows.de/cdn-cgi/l/email-protection#64100b060d0517240016130d0a000b13174a0001).  

As always, the full source code is available on [GitHub open](https://github.com/tscholze/kotlin-kmm-kennzeichner) for anyone to use! Let's continue to work together on this exciting tinkering project. Here's to the next dozen! Thank you for your continued interest.