---
layout: post
title: Binner Makes Workshop Parts Organization Easy
date: 2025-04-16 08:00:20 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---



![](https://hackaday.com/wp-content/uploads/2025/04/2025-02-25_20-08-11.png?w=800){sizes="auto, (max-width: 800px) 100vw, 800px" srcset="https://hackaday.com/wp-content/uploads/2025/04/2025-02-25_20-08-11.png 1991w, https://hackaday.com/wp-content/uploads/2025/04/2025-02-25_20-08-11.png?resize=250,127 250w, https://hackaday.com/wp-content/uploads/2025/04/2025-02-25_20-08-11.png?resize=400,204 400w, https://hackaday.com/wp-content/uploads/2025/04/2025-02-25_20-08-11.png?resize=800,408 800w, https://hackaday.com/wp-content/uploads/2025/04/2025-02-25_20-08-11.png?resize=1536,783 1536w"}



We've all had times where we knew we had some part but we had to go searching for it all over as it wasn't where we thought we put it. Organizing the numerous components, parts, and supplies that go into your projects can be a daunting task, especially if you use the same type of part at different times for different projects. It helps to have a framework to keep track of all the small details. [Binner is an open source project](https://github.com/replaysMike/Binner) that aims to allow you to easily maintain a database that can be customized to your use.

[![dashboard of binner UI](https://hackaday.com/wp-content/uploads/2025/04/binner-v2.4.png?w=400){sizes="auto, (max-width: 400px) 100vw, 400px" srcset="https://hackaday.com/wp-content/uploads/2025/04/binner-v2.4.png 701w, https://hackaday.com/wp-content/uploads/2025/04/binner-v2.4.png?resize=250,250 250w, https://hackaday.com/wp-content/uploads/2025/04/binner-v2.4.png?resize=400,400 400w, https://hackaday.com/wp-content/uploads/2025/04/binner-v2.4.png?resize=624,625 624w"}](https://hackaday.com/wp-content/uploads/2025/04/binner-v2.4.png)In a recent video for DigiKey, \[Byte Sized Engineer\] used Binner to track the locations of his components and parts in his freshly organized workshop. Binner already has the ability to read the labels used by well-known electronics suppliers via a barcode scanner, and uses that information to populate your inventory. It even grabs quantities and links in a datasheet for your newly added part. The barcode scanner can also be used to retrieve the contents of a location, so with a single scan Binner can bring up everything residing at that location.

Binner can be run locally so there isn't the concern of putting in all the effort to build up your database just to have an internet outage make it inaccessible. Another cool feature is that it allows you to print labels, you can customize the fields to display the values you care about.

The project already has future plans to tie into a "smart bin" system to light up the location of your component --- [a clever feature we've seen implemented in previous setups](https://hackaday.com/2021/01/05/the-last-component-storage-system-youd-ever-need/).

[Article Source](https://hackaday.com/2025/04/16/binner-makes-workshop-parts-organization-easy/).
{: .center-text}

