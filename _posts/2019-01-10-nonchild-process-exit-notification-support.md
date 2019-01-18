---
layout: post
title: Non-Child Process Exit Notification Support
date: 2019-01-10 12:30:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

<div data-history-node-id="1340286">





![](https://www.linuxjournal.com/sites/default/files/nodeimage/story/bigstock--219943504_4.jpg)





by [Zack Brown](https://www.linuxjournal.com/users/zack-brown "View user profile.")





**Daniel Colascione** submitted some code to support processes knowing whenothers have terminated. Normally a process can tell when its own childprocesses have ended, but not unrelated processes, or at least nottrivially. Daniel's patch created a new file in the /proc directory entryfor each process—a file called "exithand" that is readable by any otherprocess. If the target process is still running, attempts to`read()` itsexithand file will simply block, forcing the querying process to wait.When the target process ends, the `read()` operation will complete, and thequerying process will thereby know that the target process has ended.

It may not be immediately obvious why such a thing would be useful. Afterall, non-child processes are by definition unrelated. Why would the kernelwant to support them keeping tabs on each other? Daniel gave a concreteexample, saying:

Android's lmkd kills processes in order to free memory inresponse to various memory pressure signals. It's desirable to wait until akilled process actually exits before moving on (if needed) to killing thenext process. Since the processes that lmkd kills are not lmkd's children,lmkd currently lacks a way to wait for a process to actually die after beingsent SIGKILL.

Daniel explained that on **Android**, the**lmkd** process currently would simplykeep checking the proc directory for the existence of each process it triedto kill. By implementing this new interface, instead of continually pollingthe process, lmkd could simply wait until the `read()` operation completed,thus saving the CPU cycles needed for continuous polling.

And more generally, Daniel said in a later email:

I want to get pollingloops out of the system. Polling loops are bad for wakeup attribution, badfor power, bad for priority inheritance, and bad for latency. There's noright answer to the question "How long should I wait before checking\$CONDITION again?". If we can have an explicit waitqueue interface tosomething, we should. Besides, PID polling is vulnerable to PID reuse,whereas this mechanism (just like anything based on struct pid) is immuneto it.

**Joel Fernandes** suggested, as an alternative, using**ptrace()** to get theprocess exit notifications, instead of creating a whole new file under/proc. Daniel explained:





[Go to Full Article](https://www.linuxjournal.com/content/non-child-process-exit-notification-support)

Article Source [here](https://www.linuxjournal.com/content/non-child-process-exit-notification-support). My TT-RSS Published Articles Feed [here](https://app.slashetc.us/tt-rss/public.php?op=rss&id=-2&view-mticles&key=jfaea95713919339fe5).
{: .center-text}

