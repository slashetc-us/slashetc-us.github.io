---
layout: post
title: Easily Retrieve Lost Data Using Linux's Data Recovery Process
date: 2020-07-22 22:01:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---



Data recovery is often termed as a process of retrieving data that is lost. In other words, a lost data simply means the file cannot be accessed by both the user and the system. It is a nightmare that no tech-savvy would want to experience. This is because Data recovery also implies that there are chances of losing revenue, time, confidence, documentations, contacts, client information, and much more. 





[![](https://1.bp.blogspot.com/-YXi6JybDI2w/XxWKCxyipVI/AAAAAAAAFrM/teucN5TjwYMlUbb_3zDIOrs2q7eoJ4hJACLcBGAsYHQ/w400-h266/Data-Recovery.jpg)](https://1.bp.blogspot.com/-YXi6JybDI2w/XxWKCxyipVI/AAAAAAAAFrM/teucN5TjwYMlUbb_3zDIOrs2q7eoJ4hJACLcBGAsYHQ/s1920/Data-Recovery.jpg)









Everything in your work process is going fine until one day, your system starts showing glitches and the next thing you know is that you\'ve lost data due to system crash or corrupted hardware. 





\





However, the cloud-saving has made it a bit easier since the data gets synchronized. But, the same cannot be said for other types of data. So at this point, what does one tech-savvy person do? [Linux](https://linuxblog.darkduck.com/search/label/linux) is the answer. Find out more about how Linux has made data recovery easy![]{#more}



------------------------------------------------------------------------

Introduction to Live Distribution
---------------------------------



[Linux OS](https://linuxblog.darkduck.com/search/label/operating%20system) has always been unique in making sure whether Linux distribution is present in the system before its installation. What it does is produce live instances without making any changes in the hard drive. 





\





For example, if the system is built on Windows OS then even using Linux won\'t change the configuration. This is helpful in the part of [Data recovery](https://www.uaetechnician.com/hard-drive-data-recovery.html) because by accessing Linux Live tools, you can make copies of data and directories. 



Recovery Process in Linux
-------------------------



The Data recovery process gets a bit tricky but not when you learn to understand what\'s going on. Let\'s take an example of a system that is built for Windows, and suddenly faces an unexpected crash. 





\





Now the crash, unfortunately, turns out to be bad, as in it won\'t boot at any cost. But, you have important files in the internal drive. Here\'s how you can retrieve in. 





\





-   The first you can do is burn the Linux distribution platform using a boot tool into a [flash drive](https://linuxblog.darkduck.com/search/label/usb). Now, use the same flash drive to run into the system. Click on the option called \"[Ubuntu](https://linuxblog.darkduck.com/search/label/ubuntu)\" when the prompt appears. You\'ll be redirected to the Live Instance then you can find the location of the drive and enter a set of commands. 
-   When the Live Instance is running, use the following command to locate drives. Also, the command mentioned below will list out all the drives that are connected to the system.





 *sudo sfdisk -1*





\





Following are the results of the drives after using the above-mentioned command:





\








>
Disk /dev/loop14: 208.76 MiB, 218877952 bytes, 427296 sectors
>

>

>
Units: sectors of 1 \* 512 = 512 bytes
>

>

>
Sector size (logical/physical) : 512 bytes / 512 bytes
>

>

>
I/O size (minimum/optimal) : 512 bytes / 512 bytes
>

>

>
\
>

>

>
Disk /dev/loops15: 65.5 MiB, 68206592 bytes, 133216 sectors
>

>

>
Unit: sectors of 1 \* 512 = 512 bytes
>

>

>
Sector size (logical/physical): 512 bytes / 512 bytes
>

>

>
I/O size (minimum/optimal): 512 bytes / 512 bytes
>

>

>
\
>

>

>
Disk /dev/sdj: 931.53 GiB, 1000204883968 bytes, 1953525164 sectors
>

>

>
Disk model: External USB 3.0 
>

>

>
Unites sectors of 1 \* 512 = 512 bytes
>

>

>
Sector size (logical/physical):  512 bytes / 512 bytes
>

>

>
I/O size (minimum/optimal): 512 bytes / 512 bytes
>

>

>
Disklabel types: dos
>

>

>
Disk identifier 0x54daae13 
>






Mounting Process
----------------



After imprinting the code, the next process is to mount the drives of the system that has crashed. Now, if you have a single drive then the process of mounting will be relatively easy. However, if you have several drives then it can take you quite some time to mount them all until you find the data you\'re looking for. So, to make the data accessible or in short, to mount, here\'s what you need to do:



#### Steps 



The first thing you need to do is to launch a terminal window. There, you must make a new directory using the *sudo mkdir /data* command. Now, that you have made a directory, you can mount the desired drive in there. 





\





The next thing you need to do is assess the type of drive it is. For instance, if the drive is made of NTFS and also exists in the system folder of dev/sdb,  then here\'s how the command will be like:





\





*sudo mount -t ntfs -3g /dev/sdb1 /data -o force*





\





Now, the reason why the number 1 is in the command is because of the first partition. Assuming that your data exists in the first partition, you can write the code. However, you\'d most likely have to go through a number of testings with the command. For more partitions, you can simply replace the number 1 with as many partitions you think has the data. For example let\'s take 2;





\





*sudo mount -t ntfs -3g /dev/sdb2 /data -o force*





\





After you\'ve mounted all the drives, they will be placed in the new-made directory. From there, you can simply open them as you like, using the file manager. Or, you can also refer to command lines for opening specific folders.  Following are the folders you will probably see after mounting: 





\





    1. Document & Settings





    2. System Volume Information 





    3. WINDOWS 





    4. Program Files 



Dealing with the Data
---------------------



Now that you know which folder consists of the data you want to save, simply copy them. Use a [USB drive](https://goo.gl/rej5t) to do the process. Then, click on the option called \"Entry\" located in the File Manager\'s left side and then proceed to mount it. 





\





Now, right-click on the folder that has the data and then select the \"Copy\" option. Go to the USB Drive that you\'ve recently installed and then paste it. Let it past for as long as it takes which depends on how many folders you want to paste. It can take up to several minutes to finish. Once it is complete, eject the USB Drive 





\





Now, you\'ve successfully recovered your data using Linux Distribution. For instance, like this, you can use the above-mentioned process to get access to the data you thought you almost lost. 



Ddrescue Data Recovery Tool 
----------------------------



There are other ways to retrieve lost data using Linux too. For example, you can use a GNU License holder tool like Ddrescue to get Data recovery. If you experience a reading error of your data, use this tool. 





\





From any hard drive or a cd rom, Ddrescue copies the desired file and places them into either an internal device or external drive. No matter how bad the disk error is, this tool is helpful in getting your desired data back to you. Additionally, it also keeps a track of files so that the redundancy can be reduced during the recovery process. 



SafeCopy 
---------



The Linux based recovery tool is SafeCopy which has the same mechanism as the previous tool. It is efficient in retrieving data from a fully damaged drive. The programming language C has a huge role to play in getting data back. 





\





One unique thing about this tool is that it works smoothly in the background. So, even if you are working on something important, you won\'t face a glitch or lag or lowered system performance. 





\



------------------------------------------------------------------------



*This is a guest post by Perwez Haider*





\





[![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?d=yIl2AUoC8zA)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:yIl2AUoC8zA) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?d=63t7Ie-LG7Y)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:63t7Ie-LG7Y) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?i=B8vdfykoSJM:ZFpB3PE0Ns4:-BTjWOF_DHI)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:-BTjWOF_DHI) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?d=dnMXMwOfBR0)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:dnMXMwOfBR0) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?i=B8vdfykoSJM:ZFpB3PE0Ns4:F7zBnMyn0Lo)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:F7zBnMyn0Lo) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?i=B8vdfykoSJM:ZFpB3PE0Ns4:V_sGLiPBpWU)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:V_sGLiPBpWU) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?d=l6gmwiTKsz0)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:l6gmwiTKsz0) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?i=B8vdfykoSJM:ZFpB3PE0Ns4:gIN9vFwOqvQ)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:gIN9vFwOqvQ) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?d=TzevzKxY174)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:TzevzKxY174) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?d=7Q72WNTAKBA)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:7Q72WNTAKBA) [![](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?d=qj6IDK7rITs)](http://feeds.feedburner.com/~ff/LinuxNotesFromDarkduck?a=B8vdfykoSJM:ZFpB3PE0Ns4:qj6IDK7rITs)



![](http://feeds.feedburner.com/~r/LinuxNotesFromDarkduck/~4/B8vdfykoSJM)

[Article Source](http://linuxblog.darkduck.com/2020/07/easily-retrieve-lost-data-using-linuxs.html).
{: .center-text}

