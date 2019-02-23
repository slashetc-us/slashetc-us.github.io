---
layout: post
title: Yubikey Full Disk Encryption with LUKS
date: 2018-10-23 20:07:03.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Bash tips
- CLI Fu
- Debian
- General Computing
- Linux
- Ubuntu
tags: []
meta:
  _edit_last: '1'
  _responsive_layout: default
  responsive_meta_box_designation: ''
  responsive_meta_box_facebook: ''
  responsive_meta_box_twitter: ''
  responsive_meta_box_googleplus: ''
  responsive_meta_box_text_linkedin: ''
  _oembed_efea5193051e44c4d56bc1fb3ab2e279: "{{unknown}}"
  _wpas_done_all: '1'
---
So I finally got my self a yubikey, and what was the first thing I tried with it - luks full disk encryption. Yea.. better said than done. After a bit of research (read: failed attempts) I found this <a href="https://github.com/cornelinux/yubikey-luks">repo</a> on github, seemed promising. However it didn't work.. what gives.. after a bit of research into the repo I found a few pull requests that fix a couple of things. I went ahead and cloned the repo, made the changes, and what do you know it works. So I decided to fork the project and added some of the changes <a href="https://github.com/finlstrm/yubikey-luks">here</a> where I'll attempt to maintain it. See below the README.md.
I've tested this on Debian Sid, so your mileage may vary. If it breaks, you get to keep both halfs :).
Pull requests welcome!!!
<!--more-->

## Yubikey for LUKS

This package is inspired and based on https://github.com/tfheen/ykfde.
This package is forked from: https://github.com/cornelinux/yubikey-luks
This enables you to use the yubikey as 2FA for LUKS.
The Password you enter is used as challenge for the yubikey
The keyscript allows to boot the machine with either
the password and the Yubikey or with a normal password
from any key slot.

luksSuspend/luksResume integration is inspired and based on https://github.com/zhongfu/ubuntu-luks-suspend

### initialize Yubikey

Initialize the Yubikey for challenge response in slot 2

    ykpersonalize -2 -ochal-resp -ochal-hmac -ohmac-lt64 -oserial-api-visible

### install package

Build the package (without signing it):

    make builddeb NO_SIGN=1
    Install the package:
    dpkg -i DEBUILD/yubikey-luks_0.*-1_all.deb

### Assign a Yubikey to an LUKS slot

You can now assign the Yubikey to a slot using the tool

    yubikey-luks-enroll

Technically this is done by writing the response to your password (1st factor knowledge) created by the Yubikey (2nd factor possession) to a key slot. Admitted - If the attacker was able to phish this response which looks like this: ```bd438575f4e8df965c80363f8aa6fe1debbe9ea9``` it can be used as normal password.

If you set CONCATENATE=1 option in the file /etc/ykluks.cfg then both your password and Yubikey response will be bundled together and written to key slot:

    passwordbd438575f4e8df965c80363f8aa6fe1debbe9ea9

If you set HASH=1 option in the file /etc/ykluks.cfg then your password will be hashed with sha256 algorithm before using as challenge for yubikey:

    printf password | sha256sum | awk '{print $1}'
    5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8

### Changing the welcome text

If you want to change the welcome text a.k.a. the passphrase prompt you can edit the file /etc/ykluks.cfg. After changing this file, you need to run:

    update-initramfs -u

so that the changes get transferred to the initramfs.

### Enable yubikey-luks-suspend module

You can enable ```yubikey-luks-suspend module``` which allows for automatically locking encrypted LUKS containers and wiping keys from memory on suspend and unlocking them on resume by using luksSuspend, luksResume commands.

    systemctl enable yubikey-luks-suspend.service

### Open LUKS container protected with yubikey-luks

You can open LUKS container protected with yubikey-luks on running system

    yubikey-luks-open

### Enable yubikey-luks initramfs module

In order to use yubikey-luks for unlocking LUKS encrypted volume at boot you must append keyscript=/usr/share/yubikey-luks/ykluks-keyscript to the /etc/crypttab file. Example:

	cryptroot /dev/sda none  luks,keyscript=/usr/share/yubikey-luks/ykluks-keyscript

After changing this file, you need to run

  update-initramfs -u

so that the changes get transferred to the initramfs.

Alternatively you may add keyscript=/sbin/ykluks-keyscript to your boot cmdline in cryptoptions. Example:

	cryptoptions=target=cryptroot,source=/dev/sda,keyscript=/sbin/ykluks-keyscript

Updated: 20190223 - Not sure how I missed this, but it was working without it up until about a month ago with a dist-upgrade (kernel update and whatnot). My machines don't seem to have this at all and still worked - strange.... My version of the [yubikey-luks](https://github.com/finlstrm/yubikey-luks) package will add a line to primary drive when enabling this.

### Manage several Yubikeys and Machines

It is possible to manage several Yubikeys and machines. You need to use privacyIDEA to manage the Yubikeys and the privacyIDEA admin client to push the Yubikey responses to the LUKS slots.

See https://github.com/privacyidea/privacyideaadm and https://github.com/privacyidea/privacyidea
