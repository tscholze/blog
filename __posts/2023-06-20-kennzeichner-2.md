---
title: 'Devlog #2 Kennzeichner - Excel knows geo locations as long it isnt Berlin'
date: '2023-06-20'
abstract: 'Awesome to see, that Excel is far more than a sheet renderer. It has also rundimentary geo information build in'
---

In the last dev log entry, I described my less than successful attempt to use Microsoft's new Bing AI chat to create a data base for our new tinkering project, the Kennzeichner app. Excel does the job better - if you don't live in Berlin.

For those of you who are still unfamiliar with this series of articles - you've missed this so far, but you can of course read up on it afterwards:

* Developer's Diary: [Kennzeichner #1 using Bing AI chat as data basis](https://tscholze.github.io/blog/2023-06-19-kennzeichner-1.html)

## More Bing AI attempts

Thanks to the several feedbacks from the community around the prompt creation I was able to make further progress. Unfortunately, all of them resulted in wrong statements, like the already mentioned sending of mails with the created data, or that data is output unabridged in the browser, but still cut off in the middle by "...".

What surprised me the most was that I often got different answers for the same prompt. Some stopped processing right away, others continued to the one already mentioned. I have not been able to present a pattern of when which statement came.

## Retro feelings with CSV

After all the attempts, I finally wanted to make progress, so I looked around on GitHub for appropriate data. There were quite a few attempts there that tried exactly the same thing as I did and by all appearances were unsuccessful as well.

In view of this, I decided to start with the smallest part and then expand the respective data sets step by step.

On GitHub I found a \*.yaml-based list of license plates ([offene-daten](https://github.com/offene-daten/kennzeichen/blob/master/de.yaml)/[kennzeichen/de.yaml](https://github.com/offene-daten/kennzeichen/blob/master/de.yaml)) as well as the \*.csv file [Octoate](https://github.com/Octoate/KfzKennzeichen/blob/master/src/de/octoate/kfzkennzeichen/data/kfzkennzeichen-deutschland.csv)/[KfzKennzeichen/kfzkennzeichen-deutschland.csv](https://github.com/Octoate/KfzKennzeichen/blob/master/src/de/octoate/kfzkennzeichen/data/kfzkennzeichen-deutschland.csv), which is more up-to-date.

My criterion, which data state is more current, was the license plate "SMÜ" (Schwabmünchen, Bavaria). This license plate is the "newest" one I know of. How current or complete the lists really are, I can't check that way, of course. For the use in our tinkering project this is sufficient, especially since Excel handles the classical CSV (Comma separated values) format well and thus makes it easier for me to extend the data sets.

## Excel can do geography

I've been a software developer for many years, but I've never had much contact with Excel apart from conditional formatting or the sum function.

So I was all the more surprised to find out that Excel has a geography function that can calculate more detailed information for given locations. Beside the standards like the calculation of coordinates to places, Excel can also show the "leader" aka the mayor, the number of people living there or the area of the region.

### Nobody likes to go to Berlin

Excel is also just a software and caused a smile or two when I used it. For example, Excel cannot do anything with the term "Berlin". Thus the program can find neither coordinates nor other meta information about it. The reason for this is a mystery to me. My first assumption, that Excel can't handle city states like Berlin, Bremen and Co. was not confirmed, because for example Hamburg could be resolved. Also the appending of more delimiting words like "Germany" did not bring any success here.

## Manual work: license plates are not always places

Here it came back to my mind that license plates are not always related to cities. Especially in rural regions, at least in Bavaria, entire areas are grouped together. An example for this is the license plate "OAL", which stands for "Landkreis Ost Allgäu" and therefore contains more than just one city. For such data Excel has no information at all. However, in my eyes this is justifiable and less surprising than the circumstance with Berlin.

So I had to do some manual work and for a few places or areas I had to find the corresponding information like the coordinates myself.

[![German license plates in Excel](https://www.drwindows.de/news/wp-content/uploads/2024/04/Screenshot-2023-04-06-at-11.21.34.png)](https://www.drwindows.de/news/wp-content/uploads/2024/04/Screenshot-2023-04-06-at-11.21.34.png)

## Final product: JSON file

After all the efforts and attempts, I was finally able to transform my Excel spreadsheet [using a web service](https://csvjson.com/csv2json) into a machine-readable \*.json file and - this is where Microsoft comes into play again - put it on GitHub as a "quasi backend".

Attention here: If, for example, you only store an \*.json file in Git and call it via the file browser of the repository and use this URL, it will in most cases not be usable by the apps, because the http response does not have the expected MIME type "application/json", but only "text/plain". A solution for this is to deposit the file in a GitHub page. Then it will also work with the apps.

Both files can be found in the [repository of the identifier under /data](https://github.com/tscholze/kotlin-kmm-kennzeichner/tree/main/data).

## What is the next step?

Next up is the actual development. Starting with setting up a rudimentary Kotlin Multiplatform Mobile (KMM) project using Android Studio and testing whether the included Android app starts as well as iOS app. In addition, there are administrative tasks around the GitHub repository of this project to have a clean starting point eventually.

If you want to contribute, there are several options available. On the one hand everything around Excel. The spreadsheet needs quite a bit more professional work and certainly has some room for improvement. Since the GitHub state is a bit ahead of the diaries, any help around Kotlin, Android and Jetpack Compose is welcome.