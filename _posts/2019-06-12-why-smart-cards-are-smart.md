---
layout: post
title: Why Smart Cards Are Smart
date: 2019-06-12 11:30:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---







![](https://www.linuxjournal.com/sites/default/files/nodeimage/story/bigstock-Encryption-And-Data-Protection-296021644.jpg)





by [Kyle Rankin](https://www.linuxjournal.com/users/kyle-rankin "View user profile.")





*If you use GPG keys, learn about the benefits to storing them on a smart card.*

GPG has been around for a long time and is used to secure everythingfrom your email to your software. If you want to send an email tosomeone and be sure that no one else can read or modify it, GPGsigning and encryption are the main method you'd use. Distributions useGPG to sign their packages, so you can feel confident that the ones youdownload and install from a package mirror have not been modified fromtheir original state. Developers in many organizations follow the bestpractice of GPG-signing any code they commit to a repository. By signingtheir commits, other people can confirm that the changes that claim tocome from a particular developer truly did. Web-based Git front endslike GitHub and GitLab let users upload their GPG public keys, so whenthey do commit signed code, the interface can display to everyone elsethat it has been verified.

Yet, all of the security ultimately comes down to the security ofyour private key. Once others have access to your private key, theycan perform all of the same GPG tasks as though they were you. Thisis why you are prompted to enter a passphrase when you first set upa GPG key. The idea is that if attackers are able to copy your key,they still would need to guess your password before they could use thekey. For all of the importance of GPG key security, many people stilljust leave their keys in \~/.gnupg directories on their filesystem andcopy that directory over to any systems where they need to use GPG.

There is a better way. With OpenPGP smart cards, you can store your keys ona secure device that's protected with a PIN and not only store your keysmore securely, but also use them more conveniently. Although some laptops comewith integrated smart card readers, most don't. Thankfully, these devicesare available as part of multi-function USB security token devices froma number of different vendors, and *Linux Journal* has published reviews of suchproducts in the past. In this article, I discussall the reasons OpenPGP smart cards are a better choice for storingyour keys than your local filesystem.

### Reason 1: Tamper-proof Key Storage

One of the main benefits of a smart card is that it stores your GPG keyssecurely. When you store your keys on a filesystem, anyone who can accessthat filesystem can copy off the keys. On a smart card, once keys go in,they never leave, neither accidentally nor from tampering. The smart cardchips themselves are designed to be tamper-proof and resist attempts toextract key data even when someone has physical access. By putting keyson a smart card, you can have a reasonable assurance that your keys aresafe, even from a determined attacker.





[Go to Full Article](https://www.linuxjournal.com/content/why-smart-cards-are-smart)

[Article Source](https://www.linuxjournal.com/content/why-smart-cards-are-smart).
{: .center-text}

