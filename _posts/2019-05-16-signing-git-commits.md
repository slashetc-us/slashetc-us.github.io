---
layout: post
title: Signing Git Commits
date: 2019-05-16 12:00:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---







![](https://www.linuxjournal.com/sites/default/files/nodeimage/story/bigstock-Encryption-And-Cryptography-Co-252887695.jpg)





by [Kyle Rankin](https://www.linuxjournal.com/users/kyle-rankin "View user profile.")





*Protect your code commits from malicious changes by GPG-signing them.*

Often when people talk about GPG, they focus on encryption—GPG's ability toprotect a file or message so that only someone who has the appropriateprivate key can read it. Yet, one of the most important functions GPG offersis *signing*. Where encryption protects a file or message so that only theintended recipient can decrypt and read it, GPG signing proves that themessage was sent by the sender (whomever has control over the private key usedto sign) and has not been altered in any way from what the sender wrote.

Without GPG signing, you could receive encrypted email that only you couldopen, but you wouldn't be able to prove that it was from the sender. But,GPG signing has applications far beyond email. If you use a modern Linuxdistribution, it uses GPG signatures on all of its packages, so you can besure that any software you install from the distribution hasn't been alteredto add malicious code after it was packaged. Some distributions even GPG-signtheir ISO install files as a stronger form of MD5sum or SHA256sum to verifynot only that the large ISO downloaded correctly (MD5 or SHA256 can do that),but also that the particular ISO you are downloading from some random mirroris the *same* ISO that the distribution created. A mirror could change thefile and generate new MD5sums, and you may not notice, but it couldn'tgenerate valid GPG signatures, as that would require access to thedistribution's signing key.

### Why Sign Git Commits

As useful as signing packages and ISOs is, an even more important use of GPGsigning is in signing Git commits. When you sign a Git commit, you can provethat the code you submitted came from you and wasn't altered while you weretransferring it. You also can prove that you submitted the code and notsomeone else.

Being able to prove who wrote a snippet of code isn't so you know who toblame for bugs so the person can't squirm out of it. Signing Git commits isimportant because in this age of malicious code and back doors, it helpsprotect you from an attacker who might otherwise inject malicious code intoyour codebase. It also helps discourage untrustworthy developers fromadding their own back doors to the code, because once it's discovered, the badcode will be traced to them.

### How to Sign Git Commits

The simplest way to sign Git commits is by adding the `-S` optionto the `gitcommit` command. First, figure out your GPG key ID with:





[Go to Full Article](https://www.linuxjournal.com/content/signing-git-commits)







![](http://feeds.feedburner.com/~r/LinuxJournalHowtos/~4/p-bzuTsLnu0)

[Article Source](https://www.linuxjournal.com/content/signing-git-commits).
{: .center-text}

