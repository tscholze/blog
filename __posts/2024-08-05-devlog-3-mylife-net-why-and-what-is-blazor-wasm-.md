---
title: 'Devlog #3 MyLife.NET - Why and what is Blazor WASM?'
date: '2024-08-05'
abstract: 'Using the magic of .NET and Blazor without needed a server'
---

## Clarification: I am a beginner

Like all our tinkering, _MyLife.NET_ ([GitHub Repository](https://github.com/tscholze/dotnet-mylife)) is for learning, experimenting and making mistakes. Especially in this article, which is about web development, I would like to explicitly point this out once again. I'm a complete beginner when it comes to things like JavaScript, CSS or even simple HTML, and I've given these topics a wide berth so far - simply because I'm not interested in the subject. I am more at home in areas such as mobile or server development. Nevertheless, I would like to use the development time of MyLife.NET to get a taste of it. I am always open to suggestions for improvement on GitHub and I look forward to learning from you.

## What is Blazor WASM?

First, let's clarify what exactly [Microsoft .NET Blazor](https://dotnet.microsoft.com/en-us/apps/aspnet/web-apps/blazor) is.Blazor is an ASP.NET web framework for creating reactive web applications. These websites are made up of so-called Razor components, which have been known for many years. These components can either be individual parts of a page, such as a text field, but can also represent entire pages with URL routing. Since Blazor consists of _Razor_\ components, this also explains why the file extension \*.razor instead of \*.blazor seems illogical to me at first glance.

Since Blazor is part of .NET, it allows developers who want to avoid JavaScript to still be active in web development and use the knowledge they have built up in the C# and . NET ecosystem can also be used here.

### Interjection: What is WASM?

WebAssembly, or [WASM](https://developer.mozilla.org/en-US/docs/WebAssembly) for short, is an open web standard with the aim of being able to execute bytecode with almost native performance in the browser and being independent of specific programming languages. In addition to the core implementation in the browser, there are also variants such as Wasmer that do not require a browser.In addition to C# and F#, as addressed in the case of Blazor for .NET, other programming languages such as Rust, Go, Python, TypeScript, Kotlin or C++ can also be considered.WASM is open source and is available at

### Server or WASM?

There are several ways to publish websites created with Blazor: On the one hand, the traditional approach with an active web server in the background, where all logic is executed on remote machines. This type of website provision is the forefather and is also known from PHP, Ruby and Co.In our case, however, we don't want to operate any servers at the beginning and don't actually want to pay anything. For this reason, we need something static. This means that the web server only has to process simple HTTP requests, but is not allowed to actively and dynamically execute any other website logic.

This restriction allows us to run the corresponding Blazor WASM pages on services such as [GitHub Pages](https://pages.github.com/) or Codeberg Pages without having to worry about administration or billing at the end of the month. This means that the client, in our case the browser, has to do the thinking and load the required libraries when the website is visited for the first time, which in turn slows down the initial display of content considerably. For us, however, this is irrelevant for the time being.

In addition to the point mentioned above, it is also not possible to transfer logics 1:1 from a server-based application to logics running in the browser. This caused confusion during my experiment, which I will discuss later in the article.

## The beginning of MyLife.Blazor.Wasm

Creating the Blazor project within the Visual Studio Solution was easy as it only took a few clicks. The thought process of what type of Blazor template to choose was a more arduous time here. Here it pays off to set the development environment to English - at least for me, the terminology and description texts were then easier to understand.

[![](https://www.drwindows.de/news/wp-content/uploads/2025/05/Screenshot-2024-05-17-195421.png)](https://www.drwindows.de/news/wp-content/uploads/2025/05/Screenshot-2024-05-17-195421.png)

Since I didn't want the built-in styling that the example page always comes with, I finally decided to use the "Empty Blazor Web Assembly" template. Last but not least, I linked _MyLife.Core_ to the project now called _MyLife.Blazor.Wasm_ in order to have access to the models, services and helpers created in the first parts of the article series.

## Design - very simple please

Long-time readers of Dr. Windows will notice that I also used my standard styling for the MyLife.NET website, based on a class-less CSS template. Embarrassingly, I have to admit that I have customized this file over the years to such an extent that I can no longer find my original inspiration - except for the fact that it comes from the [Drop-In-CSS-Overview](https://github.com/webdevjeffus/drop-in-css).

My lack of in-depth knowledge of web development means that the entire styling structure is heavily trimmed for simplicity. My CSS file lacks adaptations for different browsers or even responsiveness as well as the reuse of variables or "prefixed attributes".

[![](https://www.drwindows.de/news/wp-content/uploads/2025/05/Screenshot-2024-05-17-195757.png)](https://www.drwindows.de/news/wp-content/uploads/2025/05/Screenshot-2024-05-17-195757.png)

Despite these limitations, I am satisfied with how my first attempt looks and operates on my system in my browser of choice. This is the most important thing in our tinkering. It's not for others to celebrate, but for us to enjoy it and feel like "Yes, I've achieved something!".

## CORS: My enemy who became a friend

Accessing data from a server and displaying it in the client is commonplace these days - as an app developer, this is also my daily bread and butter. For this reason, I had no doubt that it doesn't matter where my data is stored, as long as it can be accessed securely via _https_.

Poppycock when it comes to web assembly - i.e. local JavaScript! This is where CORS comes into play, which in my case prevents me from retrieving my JSON files from a server other than the one where the Blazor files are located. So when I tried to develop the feature, my console was full of crash and error messages regarding data access. Crap - this is getting unnecessarily complicated! At first glance, CORS is a major hindrance, but at second glance, it's our bastion of digital security.

### What is CORS

CORS ([Wikipedia](https://de.wikipedia.org/wiki/Cross-Origin_Resource_Sharing)) stands for "Cross-Origin Resource Sharing". It is a security policy that is implemented in all browsers. Websites can load resources such as images, other JavaScript files and the like from different domains. CORS was developed to prevent a website from accessing resources from another domain unless that domain explicitly allows it.

CORS helps to prevent malicious attacks by ensuring that only trusted domains can access resources. Without CORS, attackers could potentially steal sensitive data or execute malicious code.

### Solution: wwwroot/

If we look at the Blazor project in Visual Studio, we notice the special folder "_wwwroot/_". This folder contains all the files that are delivered as normal files. In addition to our _\*.css_\ files, it also contains the _index.html_ page, which is initially delivered when the page is visited, as well as our \*.json files, which we can then also pull in the context of WebAssembly.

Since, as mentioned, I can't address external servers, I also have to approach _MyLife.Core_ again and write an aggregation exporter for my Medium and YouTube feeds so that they can also be addressed in a WASM context.

[![](https://www.drwindows.de/news/wp-content/uploads/2025/05/Screenshot-2024-05-17-195330.png)](https://www.drwindows.de/news/wp-content/uploads/2025/05/Screenshot-2024-05-17-195330.png)

However, I still need to find a way to push the updated JSON files to the folder in question via automation. So there is still some thinking to do - but it would be boring if I didn't.

### Not yet finished with MyLife.Blazor.Wasm

Contrary to expectations, this article will not be able to cover the entire topic of Blazor or _MyLife.Blazor_ in general. Important cornerstones of development such as the CORS-safe data exchange mentioned above and the GitHub action behind it are still missing, as well as how to best structure Razor components. There are various patterns in web development, especially from the React camp. Roughly speaking, these relate to how much you should build components "standalone", whether they should come with the required CSS information or whether there should be a large stylesheet file, and other topics in which I simply want to build up more confidence before I consider them done.

In addition, there is still work to be done on caching, routing and other issues that need to be completed before I'm no longer afraid that it will break during the first test.

Am I disappointed?
-------------------

In the first few minutes after I discovered the problems via CORS, yes. Because my original plan seemed to go up in smoke. This, coupled with my aversion to web technologies, caused Visual Studio to shut down hard and the computer to stay off for the evening. Over time, I realized that this was an exciting learning and that with a little extra brain power, I was able to turn this problem into a feature and pull out new functionality like the aforementioned Content Creation Feed Aggregator.

Would you like to get involved?
--------------------------

If you want to get involved because you want to show me how to do something properly or more easily, or you want to participate for the first time in a .NET solution that is still clearly laid out, you are welcome to visit [GitHub](https://github.com/tscholze/dotnet-mylife), fork the repository, open issues with problems or hints or simply browse.