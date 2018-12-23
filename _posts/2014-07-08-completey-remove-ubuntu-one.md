---
layout: post
title: Completey Remove Ubuntu One
date: 2014-07-08 21:20:27.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories: []
tags: []
meta:
  _edit_last: '1'
  _responsive_layout: default
---
<p>Ubuntu One was a sync service a-la Dropbox. It work exactly the same. Files live in your home directory, and were synced across multiple Ubuntu PCs. Canonical recently (April) put a axe in the project and called it quits for the sync service (good riddance). I never liked the service myself. I had always used DropBox and more recently OwnCloud to sync all my files. Both have great cross-compatibility and even better is OwnCloud is open source, Ubuntu One was only half. The Client Side half of the service was open, however the Server Side was closed. That in and of itself caused a lot of issues in the Open Source Community, not only because it was not open, but was Closed Software coming from an Open Source company.</p>
<p>Rants aside for Ubuntu One. I never took the time to remove from my system due to it never bugged me. The code just sat there on my box without a peep. But since Canonical ended things, every time I log into my laptop I these annoying messages from Ubuntu telling me that Ubuntu One is now closed (you think Canonical would put an update to remove all traces of Ubuntu One by now, it's July). Any ways, if you are like me and tired of seeing these notification messages then there is a simple process to remove Ubuntu One as follows:</p>
<p>(normal disclaimers apply, If you loose data, computer explodes, what have you; I cannot be held responsible for the below code run on your system. Running the below code is done at your own risk.)</p>

    # sudo killall ubuntuone-login ubuntuone-preferences ubuntuone-syncdaemon
    # mv ~/Ubuntu\ One/ ~/UbuntuOld
    # rm -rf ~/.local/share/ubuntuone/
    # rm -rf ~/.cache/ubuntuone/
    # rm -rf ~/.config/ubuntu/
    # rm -rf ~/.config/ubuntuone/
    # sudo apt-get purge ubuntuone-* python-ubuntuone-storage* -y

<p>If you never used Ubuntu One, you can skip the backup (mv) portion of the code. If followed correctly then Ubuntu One has been successfully removed from your system. I did a reboot for good measure but is totally unneeded. Just be careful with the "rm -rf" statements in there, best to copy and paste the lines to be sure no typos.</p>
