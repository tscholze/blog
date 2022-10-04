---
title: 'My expierences with the JetBrains Space app tutorials'
abstract: 'I created a tiny chat bot to participate in the JetBrains Space app contest. Here's how it went.'
date: '2022-10-04'
---

I use GitHub during my day job and also for my spare time projects. But it is always fun to play around with other developer ecosystems. JetBrains, the company behind IDEs like IntelliJ, AndroidStudio or Rider launch quite a while ago [there "all in one solution" called "Space"](https://www.jetbrains.com/de-de/space/). I do not think it will replace YouTrack or TeamCity - but we will see and the future will tell their product roadmap.

Last month, JetBrains launched their anual [Space app contest](https://blog.jetbrains.com/space/2022/08/09/space-apps-contest/) (yes I'm late to the game) and I was looking for some real world use cases to dive deeper into learning Kotlin. 
As always, I started watching the offical Space App development episode at JetBrainsTV on YouTube. After this, I started with the [actual tutorials](https://github.com/JetBrains/space-app-tutorials). And oh my god, this was a successful but bumpy ride till the end. 

![My on this day bot](https://github.com/tscholze/kotlin-spaces-app-onthisday/raw/main/docs/chat.png)

If you want to check my tiny chat bot out, see my GitHub repository ['kotlin-space-onthisday'](https://github.com/tscholze/kotlin-spaces-app-onthisday)

## What happend

**Please keep in mind**

Before I begin, I want to clarify, that I'm a beginner in Kotlin, Ktor, Gradle and all other technologies that will be mentioned later in this post. This means, that maybe most of the items below may not happen if you an already expierenced Kotlinl, JVM or backend developer.

**Missing big picture**

What is actually a "Space app" and why it is called sometimes "Space extension"?. After some studieng and reading, a Space app is a ktor Server application which consumes data requests send by Space events, actions, messages, etc. pp. That means an app is at the same time a ktor server program and also a client program. 

In other words, it is a tiny server(less) request handler which can be stateful? In other words the SDK does not provide the container itself which is a basic ktor program, it provides wrapper methods around the HTTP and JSON handling from und to Space instance.

**Which group is targeted by the tutorials**

Sometimes a tutorial is not targeted on beginners, which is absolutely correct and reasonable to do this. 

Maybe JB could add a "skill" label to their tutorials. Like "chat box" is for beginner, others for immediates and the rest is for experts?

**GitHub and Tutorial code differs**

Maybe I missed something, but it seemed that the code is not in sync between both documentations.

**Just IntelliJ Ultimate is mentioned, not Community**

And again, for not totally newbiews this might be no issue, but I looked and looked to find the "ktor" template which is shown in the tutorial. After a while I recognized, that this seems to be an Ultimate only feature? It is even not a plugin for the Community edition.

I know that JB wants to sell their licenses, but maybe in this case would a like to the [official Ktor gradle project generator](https://start.ktor.io/) a good idea.

**Tutorial code did not build**

As always, this could be based on my lack of Kotlin knowledge. But the code from the [JB tutorial](https://www.jetbrains.com/help/space/get-started-create-a-chatbot.html#step-1-create-a-ktor-project) page did not built. There was a parameter called "payload" which was set with a "context" property, but the type did not match between the function header and the given one.

**Chat bot tutorial ends with a cliff hanger**

After I finished the working chat bot tutorial it would be awesome to have further topics handled like "how to deploy it without using [ngork](https://ngrok.com)".

For my self, I run into a huge blockade. Because now, I have finished the tutorial and also finished writing my own Space app, but how and where to host?

Again, this is a topic of which skill do you recommand for such tutorials. A pro ktor developer would have no issues finding the answers to "how" and "where" to host. I have no idea and this could stop me in participating the app contest :'(.

## Conclusion

Yes, this post points out some negative facts, IMHO, but the team is helpful, they ask us (tutorial users) what was hard or which steps could be improved.

That's why I always preach to people. If something is not that good as you assumed, provide helpful feedback to the authors!