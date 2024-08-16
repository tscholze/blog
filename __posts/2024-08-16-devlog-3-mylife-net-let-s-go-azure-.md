---
title: 'Devlog #4 MyLife.NET - Let's go to the clouds with Azure?'
date: '2024-08-16'
abstract: 'Tinkerers, Makers and Hobbyist on Azure? Does this work?
---

Finding a practical solution to a problem that you have created yourself provides a certain satisfaction. In the last article of this series on [MyLife.NET](https://github.com/tscholze/dotnet-mylife/), we discussed the limitations that Blazor.WASM brings. Now, we have a solution for this using GitHub and Azure. What a developer's dream.

## What was the problem?

[In part 3 of our article series](https://www.drwindows.de/news/entwicklertagebuch-mylife-3-wasn-blazor-wasm), I realized that a Blazor WebAssembly project, or generally any JavaScript running locally on the client, can only access data on the same domain due to the security mechanism called CORS. This was a problem because the data source for [MyLife.NET](https://github.com/tscholze/dotnet-mylife) can also be distributed - especially when it comes to accessing feeds from YouTube, Medium, and other platforms. The plan backfired. We needed a solution that provides all data in the domain-local context and, equally important, keeps it up to date.

## Time for a new JSON file!

As mentioned in the first part, I was not sure how much data should be packed into the central JSON file of MyLife.NET. In the end, I decided to create a second JSON structure and file for the required summary of my digital publications, with the name [_"cc-publications.json_](https://tscholze.github.io/dotnet-mylife/data/cc-publications.json)". This new file is created with the well-known _[Exporter.cs](https://github.com/tscholze/dotnet-mylife/blob/main/MyLife.Core/Generator/Exporter.cs)_ based on the stored Content Creation Accounts of the "_[life.json](https://tscholze.github.io/dotnet-mylife/data/life.json)_“ object and retrieved using the "_[LifeService.cs](https://github.com/tscholze/dotnet-mylife/blob/main/MyLife.Core/Services/LifeService.cs)_” available in all subprojects. Whether it makes sense to separate the context at the service level will be shown over time. At the moment, it is the simplest way to work with, but the simplest way is not always the one you should take.

Speaking of simplicity: The new structure is based on already known models and can therefore be easily integrated without having to create and maintain duplicate classes.

## GitHub Actions - the Swiss Army Knife of Automation

So now we know how to create this file, but not how to keep it up to date with the latest publications. This is where I concluded that GitHub Actions are ideal. A runner in the cloud that triggers the exporter once a day and finally places the corresponding files in the right place in the _wwwroot/_ of _MyLife.Blazor.Wasm_ using a Git commit and push command. This push in turn triggers another action that updates the [publicly accessible GitHub Page](https://tscholze.github.io/dotnet-mylife/) and thus makes the freshly maintained data available to the environment.

To further expand the thread of automation, not only is the static page on GitHub updated, but it is also directly deployed to an Azure Static Web App resource. So now we can all feel a bit like business people and get a taste of the gigantic Azure ecosystem with our little tinkering.

## Azure for tinkering? Absolutely!

In my perception, Azure has never been something for tinkerers and makers, but rather for companies and enterprises that need the big program and don't have to live in fear that a wrong click will set their credit card on fire. Well, that may still be true, but over the years Microsoft has continuously expanded the ["hobby" offering](https://azure.microsoft.com/en-in/pricing/free-services/), in other words, the "free" plan of available resources, so that nowadays even as a tinkerer, you should take another look at Azure.

One component of the hobby plan is the Static Web Apps, which are basically the Azure equivalent of GitHub Pages, but can be much more finely configured and integrated into the existing (enterprise) infrastructure if desired.

### Surprisingly easy to set up

A surprise right from the start: the setup was child's play, as long as you read exactly what it says there. The wizard that guides you through the creation of a new Static Web App is self-explanatory, but packed with information and choices that you need to understand and digest.

Words like "resource group", "pay-as-you-go", and others initially scared me off from delving deeper into the topic. This fear was taken away from me during the course of the wizard, by making it clear that - unless I want it differently - I start the web app in hobby plan mode, which is [free of charge](https://azure.microsoft.com/en-in/pricing/free-services/). Of course, this "free" comes with limitations that, as a tinkerer, I can initially ignore, such as bandwidth or data volume.

### Azure <3 GitHub

By now, it is evident that GitHub belongs to Microsoft and may even have a higher priority than the previously existing developer platform Azure DevOps aka Team Foundation. Why else would GitHub be listed before Azure DevOps? We will never know.

After selecting the repository, Azure immediately recognizes what type of Static Web App it is, in our case a Blazor app, and suggests useful configuration values. As can be seen in the screenshots, a GitHub Action file is ultimately generated, which, for example, triggers an update of the Azure Static Web App with every push to the GitHub repository. This is what I call a seamless integration of two services with real added value for the developer - and I have to emphasize again how uncomplicated and painless all of this is nowadays!

### Further Azure integrations possible in MyLife

Over time, further Azure integrations are conceivable in MyLife. One point would have been a no-brainer if Microsoft hadn't axed it: AppCenter Analytics, to be able to capture user interactions with the website and the resulting apps. So not everything is gold in the blue cloud - you just have to be aware of that in general. Nevertheless, the hobby plan still has many other products in store that could be worth looking at in the context of MyLife.

Do you have any wishes or ideas for Azure products that I should take a look at? In general, for a "non-WASM" Blazor variant, a CosmoDB paired with Azure Functions could also be conceivable.

## Do you want to contribute?

If you want to participate because you want to show me how to do something right or easier, or if you want to participate in a still manageable .NET solution for the first time, you are welcome to [visit GitHub](https://github.com/tscholze/dotnet-mylife/), fork the repository, open issues with problems or suggestions, or simply browse. We are all here to learn and, more importantly, to have fun with software development!