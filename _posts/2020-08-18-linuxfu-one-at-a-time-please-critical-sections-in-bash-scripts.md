---
layout: post
title: "Linux-Fu: One at a Time, Please! Critical Sections in Bash Scripts"
date: 2020-08-18 17:01:23 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

You normally think of a critical section --- that is, a piece of a program that excludes other programs from using a resource --- as a pretty advanced technique. You certainly don't often think of them as part of shell scripting but it turns out they are surprisingly useful for certain scripts. Most often, a critical section is protecting some system resource like a shared memory location, but there are cases where a shell script needs similar protection. Luckily, it is really easy to add critical sections to shell scripts, and I'll show you how.

Sometimes Scripts Need to Be Selfish
------------------------------------

One very common case is where you want a script to run exactly one time. If the same script runs again while the original is active, you want to exit after possibly printing a message. Another common case is when you are updating some file and you need undisturbed access while making the change.

That was actually the case that got me thinking about this. I have a script --- may be the subject of a future Linux-Fu --- that provides dynamic DNS by altering a configuration file for the DNS server. If two copies of the script run at the same time, it is important that only one of them does modifications. The second copy can run after the first is totally complete.

Atomic Files
------------

The problem is one of atomicity. You could, for example, create a temporary file with an obscure name and make sure that the file doesn't exist. But what if someone else checks at the same time? You both note the file isn't there and then you both create the file thinking you are running alone. No good.

It turns out files are a possible answer, but the locking of a file using `flock` is the right way to do it. There are two general options here: call `flock` to execute a command for you and it will acquire the lock and release it when the command completes, or you can use `flock` in your own script to protect a block of code.

The Easy Case
-------------

The idea behind `flock` is that it will put a lock on a file. The file can be open for reading or writing --- it doesn't really matter. You can get a shared lock or --- the default --- ask for an exclusive lock. You can only get an exclusive lock if there are no other locks on the file.

What file do you use? It depends. For a script, sometimes it is worth using the script file itself as the lock. That certainly makes it unambiguous although if someone copies the script to a new name, lookout. Another answer is to use a temporary file. Most systems will have `/var/lock` directory for just this purpose.

Consider a case where you have a script that works with a file. One option to the script deletes the file, but you don't want to do that if another instance of the file is using it at the time. You might do something like this:

    flock "$0"  rm "$SHAREDFILE"

This gets an exclusive lock. Other parts of the script might look like this:

    flock -s "$0" awk -f script.awk "$SHAREDFILE"

The -s means the lock is shared, so anyone else asking for a shared lock will get it, but the exclusive lock for the rm will block until the file --- which in this case is the shell script itself --- is unlocked.

Unblocking
----------

Of course, sometimes you don't want to wait forever. You can use the -n option to tell `flock` to not block. Or use -w to wait for a specified number of seconds (which does not have to be an integer). By default, if the lock doesn't work, `flock` will return a 1, but you can change that by using -E to select a different code since the command you run may also return a 1 for some reason.

Critical Section Blocks
-----------------------

Sometimes you don't want to run a single command. You want to lock up an entire portion of a script. You can do that, too. The `flock` command can take a numeric file descriptor. The file can be open for reading or writing, but it must exist. If you use the script file, that's a sure bet that it exists, of course.

You can use all the same options for blocking and time outs and you have to open up a file descriptor using bash constructs. There are several ways to do that --- and I've made [a repo of examples](https://github.com/wd5gnr/shellcs) which I'll reference below --- but I usually just use a redirect in a subshell. You can also use `exec` to get the same effect.

Have a look at [this script](https://github.com/wd5gnr/shellcs/blob/master/cs0.sh). It is a bit contrived, but it prepares a log file and then calls itself twice to create two different entries in that log file. There's no critical section protecting the log, so you'll see after the script completes how the output from both subprocesses mix together.

You can use the easy method by just having `flock` lock the script file before calling the subprocesses. That's the approach in cs.sh that you can [see here](https://github.com/wd5gnr/shellcs/blob/master/cs.sh). However, in a script like this it is usually more effective to use the block with a numeric file handle. Here's the [same example using that style of `flock`](https://github.com/wd5gnr/shellcs/blob/master/cs1.sh). The subprocesses look like this:

    ( flock 99echo Here is a log entry from A along with a directory of /etc >>"$LOGFILE"ls /etc >>"$LOGFILE"echo That is all from A >>"$LOGFILE" ) 99<"$LOCKFILE"exit 0

Of course, this is a silly example. It would be just as easy in this case to run process A, wait for it to complete, and then run process B. But that's not always the case and in a complex script, you may have work that both processes can execute in parallel, only waiting for specific critical sections. In that case, running the processes in series would be less efficient.

Singleton
---------

A very common use case for the critical section block is to stop a script from running more that one instance at any given time.

    #!/bin/bashLOCKFILE="$0"( if flock -n 99then  echo Running task  sleep 20  echo Done  exit 0else  echo You can only run one copy of this script at a time!  exit 1fi ) 99<$LOCKFILE"

This pattern is useful if you have a program that you plan to run periodically using something like cron. You might run it every minute, but --- on occasion --- the program might take more than a minute to complete. A flock barrier can prevent a large number of copies from running all at once.

In all of these cases, the file closing automatically releases the lock, so you don't have to explicitly let go of the lock. If you ever do need to release it early, the -u option is your friend.

A Few Notes
-----------

Turns out `flock` isn't part of the POSIX standard, but it is pretty common. Before Linux kernel 2.6. 12, `flock` didn't work correctly over NFS, either. If you use /var/lock or /tmp, those are very unlikely to be NFS mounted, anyway. If, for some reason, you are on a system without `flock`, mkdir is not assured to be atomic, but it usually is, so that could be another option since it also returns a status if it created a directory or not.

This locking technique is one of those things you won't need every day. But when you do need it, it is invaluable.

The opposite of running things one at a time is [running them in parallel](https://hackaday.com/2020/06/29/linux-fu-parallel-universe/). You normally don't need to unlock a lock file, since `flock` takes care of that when the file closes, but if you want to be super defensive, maybe consider cleaning up anything that needs cleaning using [a trap](https://hackaday.com/2019/08/26/linux-fu-its-a-trap/).

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/0AKXhF-SoWY)

[Article Source](https://hackaday.com/2020/08/18/linux-fu-one-at-a-time-please-critical-sections-in-bash-scripts/).
{: .center-text}

