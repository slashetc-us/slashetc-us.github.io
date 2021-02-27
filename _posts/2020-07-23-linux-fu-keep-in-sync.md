---
layout: post
title: "Linux Fu: Keep In Sync"
date: 2020-07-23 14:00:27 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

Once upon a time, computers were very expensive and you were lucky to have shared access to one computer. While that might seem to be a problem, it did have one big advantage: all of your files were on that computer.

Today, we all probably have at least a desktop and one laptop. Your phone is probably a pretty good computer by most standards. You might have multiple computers and a smattering of tablets. So what do you do to keep your files accessible everywhere? Why not run your own peer-to-peer synchronization service? Your files are always under your control and encrypted in motion. There's no central point of failure. You can do it with one very slick piece of Open Source software called [synching](https://syncthing.net/). It runs on Windows, Linux, Mac, BSD, and Solaris. There are also Android clients. We haven't tested it, but one caveat is that the unofficial iOS support sounds a little spotty.

The joke about the cloud --- that it's just other people's servers --- is on point here. Some people don't like their files sitting on a third-party server. Even if your files are encrypted or you don't care, you still have the problem of what happens if you can't reach the server --- may be on an airplane with no WiFi --- or the server goes down. Sure, Google and Microsoft don't go dark very often, but they can and do. Even if you build your own cloud, it runs on *your* servers. Syncthing is serverless: it simply makes sure that all files are up-to-date on all your end devices.

Enter Syncthing
---------------

Syncthing is written in Go --- not that you care --- and efficiently syncs directories across many devices with a number of options. The simplest setup syncs all files in a folder, on all machines, with no versioning. But there are several flavors of version control to select and you can also make folders that only publish changes or where changes will not propagate to other devices. By default data is encrypted, and optionally compressed, when synchronizing. What's more, the block exchange protocol gains efficiency as you add devices --- think of it as a private BitTorrent between your devices.

[![](https://hackaday.com/wp-content/uploads/2020/06/sync0.png?w=800){sizes="(max-width: 800px) 100vw, 800px" srcset="https://hackaday.com/wp-content/uploads/2020/06/sync0.png 1280w, https://hackaday.com/wp-content/uploads/2020/06/sync0.png?resize=250,115 250w, https://hackaday.com/wp-content/uploads/2020/06/sync0.png?resize=400,184 400w, https://hackaday.com/wp-content/uploads/2020/06/sync0.png?resize=800,368 800w"}](https://hackaday.com/wp-content/uploads/2020/06/sync0.png)

Setup
-----

Setting up Syncthing is easy. For Debian-type Linux you can follow their instructions to add a repository and install it using `apt`. There are other options for other operating systems. The only negative to the install is that it doesn't set up Syncthing as a service, which is probably something you want.

They do provide examples of how to do this on [GitHub](https://github.com/syncthing/syncthing/tree/main/etc). In my case, I had to use the `linux-systemd` files and put them in my `/etc/system.d/system` directory. The file syncthing@.service indicates that the service will run on behalf of a user. You can enable the service like this:

`systemctl --user enable syncthing.service`

The program does a good job of traversing NAT and firewalls, so I didn't have to set any of that up. Speaking of setup, the default method of running setup is to open a web browser on the localhost. By default, you must be on the local machine to access the web page, but you can change that if you want to remotely configure the system. You can also use an `ssh` tunnel to pop out on the local machine. There are some third-party GUIs and programs that can control syncthing through its API.

Coupling Devices
----------------

Devices have to know about one another. The program generates a long ID or a QR code you can use to set up one machine on the other. You really need to do this on both sides --- that is, you have to give computer A the code for computer B and give computer A's code to computer B. When you accept another computer into your device list, you can mark it as an "introducer". This will add all the computers they know and trust to your list as well.

This scheme means you need some sort of access to both computers, which is a good thing for security. If you are setting up on a headless server, though, you might need to use an `ssh` tunnel. I did this:

`ssh -L 9876:localhost:8384 my-remote-host`

Now a browser pointing to my localhost on port 9876 will appear on the syncthing administration port (8384) on the remote server. I didn't use 8384 on the local side because, of course, I was running syncthing there already.

Sharing Folders
---------------

When you create a folder you can give it a display name and a location. Those can be different on every machine or they could be the same. What ties them together is the folder ID. Any folder with the same ID that is shared between two machines will synchronize. So, for example, you could have your local `~/Documents` directory sync with a server directory called Desktop-Backups.

When you set up a folder you can turn on versioning. This keeps versions of files when a remote computer makes changes to it. It does not make versions for local changes. There are several options for versioning. The trashcan model just keeps a single copy of the old file. Simple versioning keeps a configurable number of old copies with a time stamp. There are several other choices, but those are the easiest ones.

[![](https://hackaday.com/wp-content/uploads/2020/06/sync1.png?w=400){sizes="(max-width: 400px) 100vw, 400px" srcset="https://hackaday.com/wp-content/uploads/2020/06/sync1.png 627w, https://hackaday.com/wp-content/uploads/2020/06/sync1.png?resize=250,150 250w, https://hackaday.com/wp-content/uploads/2020/06/sync1.png?resize=400,241 400w"}](https://hackaday.com/wp-content/uploads/2020/06/sync1.png)

Another feature allows you to set up folders that only send changes to remote computers or only receive them. Of course, the default is that folders both send and receive changes. You might, for example, have a master set of configuration files that you only want to change locally, but you want other computers to incorporate those changes: set the folder to only send. You'll notice the folder icons change based on your selections to include an arrow that points up or down depending on your choice.

What to Sync
------------

Once you have things set up, it is pretty addictive to start syncing directories. Sure, pictures and other documents are a no brainer. But what about 3D printer configurations? Or even your system startup scripts. It is true that the system isn't necessarily the best solution for backups, but you can use it that way, too.

When we've mentioned Syncthing to people, they often reply they would use OwnCloud or NextCloud. Each has its advantages, of course. While setting up your private cloud gives you the ability to install applications, you now have a dependency on a central server, even if it is your own.

Speaking of startup scripts, I wrote something to do that and it used Git to [synchronize and version control your bash startup](https://hackaday.com/2017/05/23/stupid-git-tricks/). That system would work well with syncthing instead of Git. If you are interested in such things, you might also want to check out [chezmoi](https://hackaday.com/2019/01/10/linux-fu-the-kitchen-sync/).

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/xO4QdtN5icg)

[Article Source](https://hackaday.com/2020/07/23/linux-fu-keep-in-sync/).
{: .center-text}

