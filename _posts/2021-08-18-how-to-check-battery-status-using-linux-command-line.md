---
layout: post
title: How to Check Battery Status Using Linux Command Line
date: 2021-08-18 16:00:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---







![How to Check Battery Status Using Linux Command Line](https://www.linuxjournal.com/sites/default/files/nodeimage/story/check-battery-status-linux-command-line.jpg)





by [Suparna Ganguly](https://www.linuxjournal.com/users/suparna-ganguly "View user profile.")





Checking the battery status through GUI is easy. Hovering the mouse cursor over the battery indicator given in the Laptop task bar simply shows the battery level. But, did you know you can find the battery status through the Linux command line as well?

Yes, there are some utilities in Linux that can be of help in this regard.

This article explains 4 different methods of checking laptop battery status using the Linux command line. So,

Why Do You Need to Check Battery Status? {#why-do-you-need-to-check-battery-status dir="ltr"}
----------------------------------------

So, why do you need to check the battery status? Knowing laptop battery health on a monthly basis is a good practice. It'll inform you about any issues your computer might have related to charging or battery life. You can get alerted earlier and take the measures required, such as charging or altering batteries.

When your PC is not active, the power management feature levels down its components to a low-power state. And also turns off the power. 

Similarly, knowing the power source, battery model name, the technology used, vendors, etc helps operate your devices better and keep work going without any hassles.

How to Check Battery Status Using Linux Command Line {#how-to-check-battery-status-using-linux-command-line dir="ltr"}
----------------------------------------------------

Follow the methods mentioned below to check battery status using the Linux command line. Check Battery Status with "upower" CommandThe command produces output 

### Check Battery Status with upower Command {#check-battery-status-with-upower-command dir="ltr"}

The upower command-line tool helps extract information related to the power source (batteries). It provides an interface to list down all the power sources of your PC or laptop.

[**Options Used with the upower Command**]{.underline}

-   --monitor: You can print a line each time a battery or power source is added by connecting --monitor to upower. It also produces outputs while the power sources are removed or changed.

-   --monitor-detail: This option prints the full power source detail whenever an event occurs.

 

[**Syntax**]{.underline}

``` {dir="ltr"}
upower -i /org/freedesktop/UPower/devices/battery_BAT0upower -i `upower -e | grep 'BAT'`upower -i $(upower -e | grep BAT) | grep --color=never -E "state|to full|to empty|percentage"
```

The above are three different ways of using acpi command to find power source information.

### Use cat and find {#use-cat-and-find dir="ltr"}

The "cat" and "find" commands also help find details about your battery and power source.

[**Syntax**]{.underline}

For the battery capacity, the syntax would be:

``` {dir="ltr"}
cat /sys/class/power_supply/BAT0/capacity
```

For more detailed battery information use the find command.





[Go to Full Article](https://www.linuxjournal.com/content/how-check-battery-status-using-linux-command-line)

[Article Source](https://www.linuxjournal.com/content/how-check-battery-status-using-linux-command-line).
{: .center-text}

