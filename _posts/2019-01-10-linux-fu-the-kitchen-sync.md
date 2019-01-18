---
layout: post
title: Linux Fu: The Kitchen Sync
date: 2019-01-10 15:01:54 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

One of the great things about Linux and similar operating systems is they are configurable. If you don’t like something, there’s a great chance you can change it easily with a few entries in a file somewhere. For example, take bash — a very popular shell by any measure. If you want a different style of command line editing, there’s an option. You want the tab key to match files regardless of case? Another option. Usually, these are set in one of your so-called profile files like `.bashrc` in your home directory.

As long as you are sitting in front of your single computer working, this is great. You customize your `.bashrc` and other files to your heart’s content and then you work in an environment that acts the way you want it to. The problem is when you have a lot of computers. Maybe you have a web server, a desktop, a firewall machine, and a few dozen Raspberry Pi computers. How do you keep all the configurations the same? Then once they are the same, how do you keep them up to date?

<span></span>

More Than One Way to Skin a Penguin
-----------------------------------

[![](https://hackadaycom.files.wordpress.com/2018/12/home.png?w=101&h=104){sizes="(max-width: 101px) 100vw, 101px" srcset="https://hackadaycom.files.wordpress.com/2018/12/home.png?w=101&h=104 101w, https://hackadaycom.files.wordpress.com/2018/12/home.png?w=202&h=208 202w"}](https://hackadaycom.files.wordpress.com/2018/12/home.png)There are actually a few ways to do it. Since configurations are typically in some sort of file, you can use just about any sort of synchronization method to make this work, it just takes a little effort. For example, you could put all your configuration files in a single directory and then place symlinks to them in the correct places within your home directory and then use `rsync` to keep that one directory consistent.

If you are familiar with Git, that’s another possible solution with the extra benefit of knowing what changed and when. In fact, in an earlier post about [strange uses for Git](https://hackaday.com/2017/05/23/stupid-git-tricks/), I talked about [my solution for this problem](https://github.com/wd5gnr/bashrc) that uses Git. However, I noticed that \[twpayne\] recently posted [`chezmoi`](https://github.com/twpayne/chezmoi) which is yet another system designed to handle this problem. It is written in Go and takes a very different approach from my system.

Features
--------

According to the project repository, `chezmoi` offers the following features:

-   You describe the desired state of files in  your home directory as declarations
-   You can use templates to generate actual files
-   Secure including integration with Bitwarden, LastPass, and some local key storage programs
-   Handles updates atomically so you can’t leave a system in a “half state”
-   Portable across several operating systems
-   Informs you of action and offers a “dry run” mode
-   Fast and works with your choice of version control systems

Sounds good. Installing it varies depending on your operating system, but you can pick that up at the repository.

In Use
------

[![](https://hackadaycom.files.wordpress.com/2018/12/redplus.png?w=106&h=106){sizes="(max-width: 106px) 100vw, 106px" srcset="https://hackadaycom.files.wordpress.com/2018/12/redplus.png?w=106&h=106 106w, https://hackadaycom.files.wordpress.com/2018/12/redplus.png?w=212&h=212 212w"}](https://hackadaycom.files.wordpress.com/2018/12/redplus.png)The binary in question is called — unsurprisingly — `chezmoi`. There are several key commands:

-   add – Put a file under `chezmoi` control
-   edit – Edit a file
-   diff – See what changes, if any, are pending
-   apply – Apply pending changes

When you add a file to the system it will put a copy of the file in `~/.local/share/chezmoi`. To be compatible with version control, `chezmoi` ignores dot files in that directory, so if you add a file like `.bashrc`, the system will automatically rename it to `dot_bashrc`.

If this was all there was to it, it isn’t terribly exciting. The only real trick here would be synchronizing the copies and `chezmoi` uses an external version control system to do that. However, what happens when you need some unique files on different sets of machines?

Unique Snowflakes
-----------------

[![](https://hackadaycom.files.wordpress.com/2018/12/snow.png?w=122&h=122){sizes="(max-width: 122px) 100vw, 122px" srcset="https://hackadaycom.files.wordpress.com/2018/12/snow.png?w=122&h=122 122w, https://hackadaycom.files.wordpress.com/2018/12/snow.png?w=244&h=244 244w"}](https://hackadaycom.files.wordpress.com/2018/12/snow.png)For example, suppose on your desktop machine you want to use a unique prompt. On the server, the firewall, and the Raspberry Pi machines you just want a simple prompt. That means each machine has to have its own `PS1` string in `.bashrc`.

You can do this by defining entries in the data section of the `chezmoi.yaml` file. Actually, you can also use JSON, TOML, or a few other formats. This file is unique per machine and allows you to substitute into template files. So my desktop’s YAML file might have a data entry for `PS1String` that contains a complex prompt for the desktop, while the Raspberry Pi ones would have something simple for that same entry.

To add a file (such as `.bashrc`) as a template, use the `-T` option to the `add` command. The template would turn `.bashrc` into `dot_bashrc.tmpl`. There are many options you can use thanks to Go’s standard template mechanism, and `chezmoi` provides variables that describe things like the computer’s host name, operating system, architecture, and user name.

Because of the templating language, you don’t just have to use custom variables. You can also create conditional sections in templates. For example, you could write something like:

    # common configexport EDITOR=vi# machine-specific configuration{{- if eq .chezmoi.hostname "work-laptop" }}# this will only be included in ~/<code>.bashrc</code> on work-laptop{{- end }}

Security
--------

`Chezmoi` also recognizes files that have private filesystem permissions and acts accordingly. The `chezmoi` directory is private, and it will mark files that should be private with a `private_` file name prefix (e.g., `private_dot_bashrc`).

[![](https://hackadaycom.files.wordpress.com/2018/12/security.png?w=139&h=139){sizes="(max-width: 139px) 100vw, 139px" srcset="https://hackadaycom.files.wordpress.com/2018/12/security.png?w=139&h=139 139w, https://hackadaycom.files.wordpress.com/2018/12/security.png?w=278&h=278 278w, https://hackadaycom.files.wordpress.com/2018/12/security.png?w=250&h=250 250w"}](https://hackadaycom.files.wordpress.com/2018/12/security.png)Presumably, you will use some version control system to handle the synchronization. In other words, when setting up or updating a machine you’d reconcile your `chezmoi` directory with the version control copy and then set anything in the configuration file and apply any changes, possibly after verifying what changes there are.

That means, of course, that your files stored will only be as private as your version control system. If you post your configuration files to a public host, then things will be exposed. One answer to that is to use the template system and do not version control the configuration file — since it is unique per machine, it shouldn’t be in that version control anyway, which also means you need it backed up another way.

For example, you might have in the configuration:

    hackaday:   password: 0xdeadbeef

This should be private. Then in the files that might be exposed to the public you’d have:

    password = {{ .hackaday.password }}

There is also an option to use encrypted configuration files using `gpg`.

Version Control
---------------

Speaking of version control, the system has some commands to do a push and pull for supported version control systems. The default is `git`, but you can change to other choices, such as `hg`, in the configuration file.

There’s also commands to just grab an entire set of configuration files or exporting a configuration. You can read about all of these commands in the project’s repository. By default, everything works on your home directory, but you can specify other target directories if you like.

In Control
----------

Overall, the program seems well thought out, but I didn’t see a compelling reason to change from my system. If it had been around when I was thinking about the problem, though, I would have probably used it instead of rolling my own. The template system is powerful, I agree, but my system gets a similar effect by picking different files based on the environment.

The unique data file in `chezmoi` is difficult to handle in version control. You’d essentially want to not version control that file at all. If your repository is private or you forbid private information in configuration files, you could version control examples of data files (that is, Raspberry Pi vs desktop) and then as part of an install you would rename the example file. However, it is more likely you would just copy an existing system’s configuration. It does have the advantage, however, that you can use a public repository safely if you set it up correctly.

My system really wants a private repository unless you don’t care about publishing your configuration files. However, all the files are under version control and the system works out what goes where with no complex template system to learn. You just have to put things in the right files. The only feature I may add to my system based on looking at `chezmoi` is an add feature since I also provide a way to sync other configuration files but right now you have to manually set it up. This step just amounts to moving it to a directory, adding a line to a file, and creating a symbolic link.

But that’s the good thing about Linux. There are many ways to do things and if you don’t like one way, you can find another. If you don’t like any of the choices, it is easy enough to create your own solution, too. In a way, that’s the problem with Linux for most normal people. They don’t want to pick between lots of choices. We just happen to be in a community that enjoys arguing over things like emacs vs vi, C vs Python, and things like that.

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/TB851fCj6dc)

Article Source [here](https://hackaday.com/2019/01/10/linux-fu-the-kitchen-sync/). My TT-RSS Published Articles Feed [here](https://app.slashetc.us/tt-rss/public.php?op=rss&id=-2&view-mticles&key=jfaea95713919339fe5).
{: .center-text}

