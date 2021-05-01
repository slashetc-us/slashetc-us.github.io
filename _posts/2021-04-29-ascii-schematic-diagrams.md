---
layout: post
title: ASCII Schematic Diagrams
date: 2021-04-29 15:30:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---



![](https://hackaday.com/wp-content/uploads/2021/04/aacircuit-feature_featured.png?w=800){sizes="(max-width: 800px) 100vw, 800px" srcset="https://hackaday.com/wp-content/uploads/2021/04/aacircuit-feature_featured.png 1200w, https://hackaday.com/wp-content/uploads/2021/04/aacircuit-feature_featured.png?resize=250,125 250w, https://hackaday.com/wp-content/uploads/2021/04/aacircuit-feature_featured.png?resize=400,200 400w, https://hackaday.com/wp-content/uploads/2021/04/aacircuit-feature_featured.png?resize=800,400 800w"}



We wondered recently about those crude ASCII schematics you see in some documentation --- are there any dedicated schematic-focused tools to draw them, or are they just hand-crafted using various ASCII-art drawing tools? To our surprise, there is such a tool. It is called [AACircuit](https://josoansi.de/aacircuit.php) and was developed by \[Andreas Weber\]. It has a history going back to 2001 when it was first introduced as ASCIIPaint. Be forewarned, however, the quality of the code may be questionable. According to the notes on \[Andy\]'s [GitHub repository](https://github.com/Andy1978/AACircuit):

**WARNING: a lot of spaghetti code ahead**
>
This code was created in 2001-2004 when I taught Borland Delphi 3 to myself. It contains many, many global variables, unstructured and undocumented procedural code and bad variable names.

[![](https://hackaday.com/wp-content/uploads/2021/04/aacircuit-duckman-arduino-uno-pinout-1.png?w=355){sizes="(max-width: 355px) 100vw, 355px" srcset="https://hackaday.com/wp-content/uploads/2021/04/aacircuit-duckman-arduino-uno-pinout-1.png 473w, https://hackaday.com/wp-content/uploads/2021/04/aacircuit-duckman-arduino-uno-pinout-1.png?resize=222,250 222w, https://hackaday.com/wp-content/uploads/2021/04/aacircuit-duckman-arduino-uno-pinout-1.png?resize=355,400 355w"}](https://hackaday.com/wp-content/uploads/2021/04/aacircuit-duckman-arduino-uno-pinout-1.png)

If you don't want to wrestle with old and sketchy object-oriented Pascal code, you're in luck. \[Chaos Ordered\] has made a Pythonized version which you can get from [his GitHub repository](https://github.com/Blokkendoos/AACircuit). We tried it out and got it working on Ubuntu in short order (after wrestling with a pycairo dependencies). This might not be everyone's cup of tea, but it has some uses now and then. While we wouldn't want to document a computer motherboard with ASCII schematics, it's great for a quick-and-dirty circuit diagrams.

Not exactly schematics, but \[Duckman\] has [some Arduino pinout diagrams](http://busyducks.com/wp_4_1/2015/11/16/ascii-art-arduino-pinouts/) he made using ASCII-art. These could be useful when pasted into source code as comments, documenting the pinouts for your project.

Do you recommend any tools for making ASCII schematics, or this just a waste of time?

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/4ZyEr1qx1yA)

[Article Source](https://hackaday.com/2021/04/29/ascii-schematic-diagrams/).
{: .center-text}

