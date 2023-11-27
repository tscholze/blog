---
title: 'Devlog #5 Kennzeichner - It is time for iOS!'
abstract: 'Now it is time for iOS, the final part of our app'
date: '2023-11-27'
---

In the previous part of this short series, we created the very first version of the Android app for the identifier ([GitHub](https://github.com/tscholze/kotlin-kmm-kennzeichner)). Now, in this part, the counterpart for iOS follows. The big question here is how compatible the core written in Kotlin is with the iPhone app to be written in Swift and SwiftUI.

For all those who are still unfamiliar with this series of articles - you've missed it so far, but you can of course read up on it later:

* Developer's Diary: [Using Bing AI chat as data basis](https://tscholze.github.io/blog/2023-06-19-kennzeichner-1.html)
* Developer's Diary #2: [Excel can do geography and doesn't like Berlin](https://tscholze.github.io/blog/2023-06-19-kennzeichner-2.html)
* Developer's Diary #3: [Shared Core - The Core of the Whole](https://tscholze.github.io/blog/2023-06-21-kennzeichner-3.html)
* Developer's Diary #4: [Lets go mobile!](https://tscholze.github.io/blog/2023-09-20-kennzeichner-4.html)
* (current) Developer's Diary #5: It is time for iOS!

## Why iOS only at the end?

There are several reasons for this. Firstly, I needed an app to test the shared core. Any unit tests would have worked too, of course, but I was driven by curiosity to experiment more with Kotlin and tools like Android Studio and to get a feel for the language.

On the other hand, there has been a lot of buzz in the Swift community over the past few months with the outlook for iOS 17 and Swift 5.9 and especially with all the implications that these now released versions have for developer life, especially in terms of software architecture and patterns for modern Swift (UI) apps.

For me personally, it should not be forgotten that I earn my daily bread with iOS and iPadOS apps. This also means that any source code I write in my spare time can (wrongly) be used as a mark of quality for my work or even my employer. In my eyes, however, there is a big difference to be made here between "fun" and "professional" code that I produce. I may be overreacting a bit here - I hope so!

[![Identifier app for iOS](https://www.drwindows.de/news/wp-content/uploads/2023/09/030-summary.png)](https://www.drwindows.de/news/wp-content/uploads/2023/09/030-summary.png)


## Technology selection
### Swift
We no longer need to discuss whether new projects should still be started with Objective-C in 2023. However, we should talk about which Swift version should be used to build new apps. In contrast to the usual semantic versioning, Swift also has incompatibilities between so-called "dot releases", for example from Swift 5.5 to Swift 5.9. It should not be neglected that these smaller version jumps also provide more help to better structure the previous source code. One example of this is the option introduced with Swift 5.9 to write your own (compiler) macros to reduce boilerplate code.

### SwiftUI
Similar to Android, there are also two ways to develop an app interface from the Cupertino camp. On the one hand, with the well-known UIKit, which has been tried and tested for decades, including its interface builders, storyboards, \*.xib files, IBOutlets and IBActions. On the other hand, there is SwiftUI, which has been increasingly developed over the last few years and, like Jetpack Compose, declares an interface using the same programming language that is otherwise used in development - i.e. Swift or Kotlin for Jetpack Compose.

Even though in my eyes SwiftUI still doesn't have the same features as UIKit despite all these years, the new framework is my first choice when it comes to new applications for the Apple ecosystem.

### Enforced dependencies

As a senior software engineer who mostly develops applications for B2B use, I always try to keep external dependencies to a minimum. For me, they represent a high risk that the application will no longer be maintainable in the future. Be it that the dependency licenses (and their dependencies) cannot be used in the context of the app or simply that I am dependent on other developers without entering into a business relationship with them. From the perspective of an Android or web developer, the following point is probably easier to get over.

The KMP-NativeCoroutines library developed by third parties is usually used in order to be able to work reasonably well with the shared core concurrently (concurrency, async await). Even though this de facto standard dependency is probably really good, it is not something that is supplied directly with KMM. KMP-NC also comes with a lot of internal dependencies that are not needed in many scenarios. Getting a security release for this in Enterprises would be a very lengthy process.

### Java in the build process

The shared core is developed in Kotlin and therefore runs on the JVM - nothing new in this respect. For this reason, the identifier has a Gradle call to rebuild the shared core as an extra build phase before the app is even built. If we had integrated the shared core as a normal dependency, which is managed via CocoaPods, this step could be omitted, but we would put on a completely different shoe.

### Mint for build tooling management

As a developer, you often need to do more than just a little compilation when building an application. In the example of the identifier, in addition to building the shared core, it is also the formatting of source code as well as the so-called "linking", i.e. indicating that certain rules in the sense of "we do not use it this way" are displayed to the developer in the development environment. Mint helps here to place all versions of the little helpers used centrally and to set them up on other computers so that they can always be restored.

One thing we have learned is that "format source code per build" works very poorly with SwiftUI Previews and reduces development speed.

## Development

### AppStore.swift

At least with the state of my KMP stack, it is relatively difficult to use the shared core, which works perfectly in Android, just as painlessly under iOS and Swift. This is due to the fact that JetBrains transpiled the shared core to Objective-C and not to modern Swift.

That's why I decided to bundle all accesses to the shared core in a wrapper called "AppStore.swift" for the time being and thus only use this as a so-called SwiftUI-typical EnvironmentObject in the iOS app, i.e. via dependency injection.

[![Identifier App for iOS](https://www.drwindows.de/news/wp-content/uploads/2023/09/Snap.png)](https://www.drwindows.de/news/wp-content/uploads/2023/09/Snap.png)

### ContentView.swift

The AppStore also controls the status of an app. Whether no data is available yet, whether data has been loaded or whether it has failed. Views, such as the topmost ContentView.swift in the hierarchy in this case, can listen to this and display corresponding interface elements. The source code example below clearly shows the if switch functionality introduced in Swift 5.9.

[![Identifier app for iOS](https://www.drwindows.de/news/wp-content/uploads/2023/09/Snap-2.png)](https://www.drwindows.de/news/wp-content/uploads/2023/09/Snap-2.png)

### RegionGridItem.swift

A clear relief compared to Android was the simple integration of a map section. No hassle with API tokens and how to create them, no wrapper in composables, it just worked. If it wasn't for changes from iOS 16 to iOS 17 in the SwiftUI syntax, which completely threw the creation of annotations, i.e. the pins on the map, out of the window again.

Nevertheless, this class is very clear and easy to understand, as no complex card logic is required.

[![Marker app for iOS](https://www.drwindows.de/news/wp-content/uploads/2023/09/Snap-5.png)](https://www.drwindows.de/news/wp-content/uploads/2023/09/Snap-5.png)

I had to explicitly set the font color, otherwise it would jump to white in dark mode. In my case, however, I wanted it to always be black to be clearly visible on the yellow background and to maintain the distant illusion of a city sign.

## First conclusion about KMP

I think it's great how much energy and expertise JetBrains and the Kotlin community are putting into the whole topic of multiplatform. I really hope that it will be successful in the long term. For me, a lot depends on how faithful they remain to their roadmap, especially to improve the interaction with iOS.

They have a good chance. Especially after the end of Visual Studio for Mac, Xamarin and .NET MAUI are history, at least for me. Flutter is too reminiscent of TypeScript and co. and is perhaps too small to be able and want to do anything with it, especially in a B2B context.

Since Microsoft has also killed the Surface Duo in addition to Visual Studio for Mac, the (developer) desire for Android apps has been stifled for me for the time being. Without a real reason for this, I'm not sure whether my next pet project will be KMP again or whether it will ultimately be developed natively for the specific platform. Time will tell.

## Next steps

This is the end for now. Not because I don't feel like pursuing KMP any further, but because JetBrains has presented a very optimistic roadmap on how to proceed with KMP. Many points, such as my problems with the Objective-C bridge, which will then be converted to Swift, are included in this list. So I'd rather wait and enjoy the beautiful KMP, especially in combination with SwiftUI.

On the other hand, the iOS app is by no means error-free and some of you may have already found bugs in the screenshots. I will probably tackle these bit by bit in order to further develop my feeling for how to implement things in SwiftUI.

What function would you like to see in the identifier? If you also want to get started with Android app development, how can we take away your fear of this topic? Let me know on Social Media. All links are in the article's footer.

As always, the [full source code is available on GitHub](https://github.com/tscholze/kotlin-kmm-kennzeichner) for everyone to use! Let's continue this exciting crafting project together. Here's to the next dozen! Thank you for your continued interest.