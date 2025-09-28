---
title: 'Devlog #5 MyLife.NET - Even more function-al Azure'
date: '2024-09-08'
abstract: 'What other Azure services could be benefitial? Let's explore the serverlessness!'
---
In the last post, we started our [MyLife.NET](https://github.com/tscholze/dotnet-mylife) journey into the world of Microsoft Azure, and since then, I have been thinking about how to further integrate the free services of the cloud into MyLife.NET. A great [comment on LinkedIn](https://www.linkedin.com/feed/update/urn:li:activity:7209136727171510272?commentUrn=urn%3Ali%3Acomment%3A%28activity%3A7209136727171510272%2C7210718128538824708%29&dashCommentUrn=urn%3Ali%3Afsd_comment%3A%287210718128538824708%2Curn%3Ali%3Aactivity%3A7209136727171510272%29) by [Jens](https://www.linkedin.com/in/jens-walter-125544140/) sparked the idea of trying out [Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview?pivots=programming-language-csharp). In this diary entry, I will share how this evaluation went.

## Use-case: Random Nickname

There's no point in exploring a new technology if you don't know what you want to use it for and how to approach the new world of the cloud. That's why, for starters, I decided to go with the simple use-case of "Give me a random nickname from the Life object".

In short, by making a GET web request to a specific URL, a randomly selected nickname will be returned in a JSON structure. Thanks to "serverless computing", we don't need to run a complete server for this. We simply throw the source code to the cloud, which will execute it in this specific case.

### Creating an Azure Function

To create a new function, it is sufficient to add a new subproject in the solution in Visual Studio. Microsoft provides various templates for this purpose. For this tinkering, the "Azure Function with HTTP Trigger" template is the one we need. The project is named `MyLife.Azure.Functions` ([GitHub](https://github.com/tscholze/dotnet-mylife/tree/main/MyLife.Azure.Functions)), as multiple functions can be included within a single project.

Thanks to the ability to include already created subprojects like "MyLife.Core", we theoretically have all the possibilities to access our digital life. This makes the actual implementation very easy, assuming we have a good solution structure and domain design.

### The Implementation

One Azure Function per C# class. This clear separation keeps the clarity alive even in large projects and ensures that the lifecycle of a cloud function is clearly visible. For our small use-case, the "`Run(...)`" method is sufficient.

In this method, I load the JSON from another endpoint and return a randomly selected list element from the data field `MyLife.Persona.Nicknames` as the response.

### Execution

It is important to distinguish whether we start it locally for development purposes or push it to the cloud later on. Locally, the Azure Functions Simulator starts, which feels and looks like a modern terminal application - including localhost URLs for each started function. These can be accessed as usual, which is possible in our simple case simply by using a browser.

If you want to start the function in the Azure Cloud, Visual Studio includes a wizard to create a new deployment and carry it out via the familiar "Right-click -> Publish" menu.

In the end, this automatically sets up resources in the specified Azure environment and uploads the function project. This is not the only option. In the Azure Portal itself, there are other options available, such as creating the necessary resources and deploying via GitHub or even writing a function entirely in the browser.

## I still have a lot to learn

The complexity of what I have built in MyLife.NET so far makes some learning curves more challenging than others. Suddenly, there are not only new frameworks to understand, but also how to handle stateless sessions or, in the case of Azure, how to avoid burning a hole in my wallet.

This is not meant to sound negative. I see it as a challenge that comes with modern software development. It also demonstrates the possibilities that C# and the .NET ecosystem offer.

### A real value is missing

Even though tinkering in the cloud is fun, I am still looking for the real value of it. Of course, if MyLife.NET were a production-ready software, it would be different. As a hobby and after-work project, it remains exciting to see how the various Azure services can be used to bring joy and benefits.

### MyLife is not cloud-ready

While tinkering, I noticed that the current structure of MyLife.NET is not cloud-ready. The location where one can define their own life and how to access it in different subprojects like Blazor, MAUI, or now Functions, exceeds the limits of the domain pattern and needs to be adjusted as soon as possible. The mixture of "when do I create life" and "when do I read it from an endpoint" severely limits me in the stateless Azure Functions experiment.

### Concerns about costs remain

Despite studying the ["Always free" section in the documentation](https://azure.microsoft.com/en-in/pricing/free-services/) and the price lists, it was not clear to me if and when Azure Functions incur costs. Since my code not only executes statically but also loads JSON files from external sources, there is data traffic that is usually not provided completely for free. Unlike Azure Static Web Apps, I couldn't find the "Hobby" or "Free" plan mentioned anywhere.

For experts in this field, the information is probably easy to find, but as a tinkerer, I find it difficult to make a meaningful statement about this. That's why, after my tests, I stopped the public deployments and even deleted all the resources. Otherwise, the Swabian in me wouldn't have had a peaceful sleep due to the worry of a glowing credit card.

## What's next?

As mentioned, the structure of how the individual subprojects interact needs to be improved to ensure better separation between creating and consuming MyLife data sources in the future. There is also the question of whether, for example, the created JSON file should be the source of all information or if it should be the created C# object. If it's the latter, where does it live, how do other subprojects access it, and how does everything actually make sense? Many open questions that need to be answered.

Before tackling this challenge, the starting signal for the MAUI app will probably be given - simply because I am currently more interested in this topic and, thanks to the fact that this is an after-work and tinkering project, I can prioritize tasks as I like. Oh, how nice.

## Want to contribute?

If you want to participate because you want to show me how to do something right or easier, or if you want to participate in a still manageable .NET solution for the first time, you are welcome to visit GitHub, [fork the repository](https://github.com/tscholze/dotnet-mylife), open issues with problems or suggestions, or simply browse around. We are all here to learn and, more importantly, to have fun with software development!