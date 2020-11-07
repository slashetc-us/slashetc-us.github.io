---
layout: post
title: How to Add a Simple Progress Bar in Shell Script
date: 2020-10-26 18:40:25 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---







![How to Add a Simple Progress Bar in Shell Script](https://www.linuxjournal.com/sites/default/files/nodeimage/story/progress-bar-2.png)





by [Nawaz Abbasi](https://www.linuxjournal.com/users/nawaz-abbasi "View user profile.")





At times, we need to write shell scripts that are interactive and user executing them need to monitor the progress. For such requirements, we can implement a simple progress bar that gives an idea about how much task has been completed by the script or how much the script has executed.

To implement it, we only need to use the "echo" command with the following options and a backslash-escaped character.

    -n : do not append a newline-e : enable interpretation of backslash escapesr : carriage return (go back to the beginning of the line without printing a newline)

For the sake of understanding, we will use "sleep 2" command to represent an ongoing task or a step in our shell script. In a real scenario, this could be anything like downloading files, creating backup, validating user input, etc. Also, to give an example we are assuming only four steps in our script below which is why we are using 20,40,60,80 (%) as progress indicator. This can be adjusted as per the number of steps in a script. For instance, a script with three steps can be represented by 33,66,99 (%) or a script with ten steps can be represented by 10-90 (%) as progress indicator.

The implementation looks like the following:

    echo -ne '>>>                       [20%]r'# some tasksleep 2echo -ne '>>>>>>>                   [40%]r'# some tasksleep 2echo -ne '>>>>>>>>>>>>>>            [60%]r'# some tasksleep 2echo -ne '>>>>>>>>>>>>>>>>>>>>>>>   [80%]r'# some tasksleep 2echo -ne '>>>>>>>>>>>>>>>>>>>>>>>>>>[100%]r'echo -ne 'n'

In effect, every time the "echo" command executes, it replaces the output of the previous "echo" command in the terminal thus representing a simple progress bar. The last "echo" command simply enters a newline (n) in the terminal to resume the prompt for the user.

The execution looks like the following:

![simple progress bar shell execution](https://www.linuxjournal.com/sites/default/files/styles/max_1300x1300/public/u%5Buid%5D/simple-progress-bar-shell.png)





[Go to Full Article](https://www.linuxjournal.com/content/how-add-simple-progress-bar-shell-script)

[Article Source](http://feedproxy.google.com/~r/LinuxJournalHowtos/~3/p8fCUC0b_qM/how-add-simple-progress-bar-shell-script).
{: .center-text}

