---
layout: post
title: "[Scripts for Lazy People] ssh-copy-id {id} {host}"
date: 2015-04-04 13:34:43.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Lazy Scripts
- Linux
tags:
- bash
- CLI
- command line
- Lazy Scripts
- Linux
meta:
  _edit_last: '1'
  _responsive_layout: default
  _syntaxhighlighter_encoded: '1'
  _wpas_done_all: '1'
---
<p>Ok I'll admit it... I can be a bit lazy. If I could script doing the dishes I would. I think it would look a bit like this:</p>

    while read SINK ; do
      clean ${SINK}
      dry ${SINK}
      mv ${SINK} /usr/share/cabnet/
      echo "Item ${SINK} has been cleaned"
    done

<p>But unfortunately this is not the case. However what I can do is script other parts of my life that I am either to lazy to remember or to lazy to remember the syntax. Today's entry I share the first in what may be an on going series of "Lazy Scripts". I will have all of my Lazy-Scripts hosted <a title="Skynet GitLab: Lazy Scripts" href="https://app.slashetc.us/gitlab/mford/general-scripts/tree/master/LazyScripts" target="_blank">here</a> on my GitLab.</p>
<p><!--more--></p>
<p>The first of my Lazy Scripts solves a problem that I can never remember the proper syntax for ssh-copy-id, even though I use it all the time.... because hate having to enter a password every time I login to a server, not to mention that is is WAY more secure that Password Authentication. This script is great as it finds what SSH keys I have in ~/.ssh/, allows selection of said SSH key, then copies to inputted server. Yes requiring entering in a password on copy, but from then on the server will allow key-pair authentication (YES, no more entering in password to login!!). The script also employs error checking. So that when you enter an invalid entry, the script will exit and force you to start over. I wanted to keep the script under 100 lines, so there is no loop having you re-enter your input. I may add that in the future, or you could add it yourself (this is the beautify&nbsp; of the GPL). Without further delay here is the code:</p>

    #!/bin/bash
    #-------------------------------------------------------------------------------
    #
    #   File:        ssh_copyid.sh
    #
    #   Description: I'm lazy, therefor only want to enter what server and rsa Id
    #                to use when setting up Pub Key Auth with SSH
    #
    #-------------------------------------------------------------------------------
    #--- Some Vars
       TITLE="Skynet SSH Copy ID Script"
       WIDTH=$(tput cols)
    #--- Get RSA ID names
       COUNT=0
       ls ~/.ssh | grep .pub &gt;/tmp/rsa.list
       while read RSA ; do
          COUNT=$(expr ${COUNT} + 1)
          echo "      ${COUNT}) ${RSA}" &gt;&gt; /tmp/RSA.LIST
       done &lt; /tmp/rsa.list
       rm /tmp/rsa.list
    #-------------------------------------------------------------------------------
    #--- Main Code
    #-------------------------------------------------------------------------------
       #--- Display Choices
       clear
       printf "%*s\n" $(((${#TITLE}+${WIDTH})/2)) "${TITLE}"
       echo " debug: count ${COUNT}"
       echo -en "
       Enter Hostname: " ; read RHOST junk
       #--- Breif Error Check
       if [ -z ${RHOST} ] ; then
          echo "   Invalid Entry, Selection cannot be blank... exiting..."
          rm /tmp/RSA.LIST ; sleep 2 ; exit 1
       fi
       #--- Further Display
       echo -e "
       I found the following RSA Keys in ~/.ssh/
       "
       cat /tmp/RSA.LIST
       echo -en "
       Please type the one you would lke to use: " ; read RSAID junk
       #--- Error Checking
       if [[ "${RSAID}" -le "${COUNT}" ]] ; then
          expr "${RSAID}" + "${COUNT}" &gt;/dev/null 2&gt;&amp;1
          IsNum=${?}
          if [ "${IsNum}" != "0" ] ; then
             echo "   Entry Must Be a Number... Exiting..."
             rm /tmp/RSA.LIST ; sleep 2 ; exit 1
          fi
       else
          echo "
       Invalid Entry, There are only ${COUNT} Choices. Yours was ${RSAID}
       Exiting..."
          rm /tmp/RSA.LIST ; sleep 2 ; exit 1
       fi
       if [ -z ${RSAID} ] ; then
          echo "   Invalid Entry, Selection cannot be blank... exiting..."
          rm /tmp/RSA.LIST ; sleep 2 ; exit 1
       fi
       #--- Where the Magic Happens
       RSAID=$(cat /tmp/RSA.LIST | grep ${RSAID} | cut -d' ' -f 8)
       echo -en "   I  will copy the public key ${RSAID} to ${RHOST}
       Is that ok? (y/n) " ; read ANSWER junk
       case ${ANSWER} in
          y|Y) echo "
       This is OK... Copying... "
             ssh-copy-id -i ~/.ssh/${RSAID} ${RHOST} ;;
          n|N) echo "
       This is NOT OK, not copying, exiting... "
             rm /tmp/RSA.LIST
             sleep 3 ; exit 1 ;;
          *) echo -en "
       INVALID ANSWER. Please hit &lt;ENTER&gt; to exit..." ; read junk
             rm /tmp/RSA.LIST
             exit 2 ;;
       esac
       exit 0
    #-------------------------------------------------------------------------------
    #--- End Script
    #-------------------------------------------------------------------------------

<p>It doesn't look as pretty here, but pop the link for my <a title="Skynet GitLab" href="https://app.slashetc.us/gitlab/mford/general-scripts/tree/master/LazyScripts" target="_blank">GitLab</a> to see it a bit more polished. As well as the latest updates on this script. Also be sure to check out the develop channel to see what I am working on next!!</p>
