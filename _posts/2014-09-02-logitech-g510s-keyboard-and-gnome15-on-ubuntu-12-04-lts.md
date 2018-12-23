---
layout: post
title: Logitech G510s Keyboard and Gnome15 on Ubuntu 12.04 LTS
date: 2014-09-02 20:00:54.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- General Computing
- Linux
- Ubuntu
tags: []
meta:
  _edit_last: '1'
  _syntaxhighlighter_encoded: '1'
  _responsive_layout: default
---
<p>Hello and once again I've gots another how to for you all. This one is going to be a little be targeted as it is for getting the Logitech G510s keyboard working with Ubuntu 12.04 LTS. It's been a long time comming for me as I have had the keyboard since December 2013 and just getting it fully funcional now without issues. All thanks go to Russo79 and the forums of <a title="Russo79.com" href="www.russo79.com" target="_blank">www.russo79.com</a>. The original forum posts can be found <a title="Russo79 Forum" href="http://www.russo79.com/forum/gnome-15/support-and-answers/263" target="_blank">here</a>. Russo's drivers are top notch, however it seems to have had some issues with the Linux Mainline Kernel 3.2.x I was using at the time in Ubuntu 12.04.4 LTS, causing Random Unresponsiveness with the keyboard. However the latest Ubuntu HWE Stack seems to have resolved the issue pushing to the Mainline Kernel 3.13.x.</p>
<p><!--more--></p>
<p>So for now I personaly saying that you should be on the latest HWE stack in order for the keyboard to work properly with Russo's drivers. If you don't know what your HWE stack status is run the following in a terminal:</p>

    hwe-support-status

<p>It should have the output:</p>

    Your Hardware Enablement Stack (HWE) is supported until April 2017.

<p>If the output is the same then push on forward, if says is no longer supported or not installed at all; please follow my post <a title="Update Ubuntu Precise to 12.04.5 with Trusty HWE" href="http://www.slashetc.us/update-ubuntu-precise-to-12-04-5-with-trusty-hwe/" target="_blank">here</a> on how to update Ubuntu 12.04.4 to 12.04.5 with Trusty HWE.</p>
<p><em>DISLCAMER</em>: All normal warnings apply here. These commands and how tos are for educational use only. While commands work and are correct to the best of my knowledge: I am not responsible for your Linux crashing, loss of data, gnomes eating your sox in the dryer or your cat walking across the keyboard. With that said… Pleas read on…</p>
<p>To install Russo's drivers, I followed is post <a title="Download Gnome15 for Ubuntu" href="http://www.russo79.com/gnome15/download/ubuntu" target="_blank">here</a>. If you happen to be upgrading from a previous repo, please see Russo's instructions.</p>
<p>Installing the Gnome15 Drivers from Russo79 if fairly straight forward. First open up a terminal and enter the following to add the PGP Public Key:</p>

    echo '-----BEGIN PGP PUBLIC KEY BLOCK-----
    Version: GnuPG v2.0.19 (GNU/Linux)
    mQENBFFeChUBCADfGbecu2/WP705CJQTlDSGpzeYL/Jev/8Vtb5g8RZhc0tQBQHz
    2SP5wGN1Wkz6UuIam1rjRAatoAFNjl4ndurlMACYeqJssS47UOdU01yxqvg546BJ
    RXLgbQNGAiu0d12ldjaWfo/LULAb2972Wvqal5ANZjkA0B8R48D7PVNVheuijtmd
    XvmlxH8iPonr2Wm/J4oxvMP3BbJL5nOJdh/FDOl2caAHU6R5bVZypizwBFycm/P3
    PUmZbbOsx3Q5vHB08auFTV4p2CqCFWts5FlWqOXwBofjTuO9qSBZ8D94MhZ2MAXO
    H98uNArQd2h1g1rGULafVVB6YNrkZ5QZ7rHhABEBAAG0O051bm8gQXJhdWpvIChQ
    YWNrYWdlIHNpZ25pbmcga2V5KSA8bnVuby5hcmF1am9AcnVzc283OS5jb20+iQE4
    BBMBAgAiBQJRXgoVAhsDBgsJCAcDAgYVCAIJCgsEFgIDAQIeAQIXgAAKCRAlQAMb
    zAMRYHQPB/96dlhY79IUZeXfo51U5+OOR+nXJQ+uBwgUxW2yqHf3QBpEzHzwWooQ
    87GH89is8vqjL6rL1HMt3P6/EdltT9pPtr/QIYPHDVgzKGw4GMpF0Ma3RS68uIK8
    RI/iyo6MzBEashIEaMEqB0TRQo8N64hk9OJ7A3wLrPc4T/jHctv0o1i+OfUKzxqw
    7BOGKB2P3a6CNAFqDVxGQSMlnJ6oj33hNFbXq1MZ0jyK7RI4fCIZ4XWRJO1eYLbq
    icHfrcq30C0UZNMOh0lFtJ+i9dbPufATKGcqOix9NItKZJ+fdGDsawByUUFelgbl
    8UWVkDPpiXFiuS2YWJAJwUZS/BzfOQwvuQENBFFeChUBCADQ20hwXNCwzghcuLf9
    1AzUVUUvPimT9QjcijtprpnhCLm84jjMHJsgGZbHmV1ZzpweeINfUwmLKQ605gXb
    vfkMdp5sXcYG3Hpe54OWGsDZzvj1fQvFYBFIyuw4u90k4rhvnl0L2RlHeXQN/yoY
    I4ckN3VVFcqlUwgHjtkM1R/X6kn3PBWJgG/ktdANn7HhmEg4tW+iSWR+I0fkivEy
    hAGvXWBjyhOkVpTE1ym6p4Oi7tjgO+mEMBMzv4RvDTDBn3zxeT9SjsBC/hNB0A9Q
    6EskaR4x9oZzjyfkRqlXFF+iA9GnDS+/lyaQ8l/8xROMsvQ80jgzXl287CHGRSIt
    36mtABEBAAGJAR8EGAECAAkFAlFeChUCGwwACgkQJUADG8wDEWBA7wf+Pm77zBJl
    7DWu+pTwh6a/L/mzWFYKFIDFJ47dr2t2gIo7DfDFZu79nSC1GJfuC3cqwwfmvTRK
    hoZud/QvaCEuqfjrvL3rSZVZ+txXUnPpnsxQzLIZBSPo/i4dNRNDAZZJUgOoTNw7
    IXCzFWB2CMpQvzYUGnQeLHLpTGYXhGFweiRpfacaC93O5d9bVIzTK1QDtVwAgVif
    kGBvGyYNwcMzGWbmuTAAhaVUrHd7UEzC+dvBmS5PAAkj1TNUKAH2bpUTCmC0C6vW
    XrcyClLiKscQqRm7dinsNIGpGcj6U72OrDWepzntK320wSpizJZdP+6OZzxPRmHE
    JZAy9UXt0Vf1Mw==
    =yaAO
    -----END PGP PUBLIC KEY BLOCK-----'| sudo apt-key add -

<p>Then enter the following to enable the Repo for Ubuntu 12.04 (precise):</p>

    sudo sh -c "echo deb http://packages.russo79.com/debian/gnome15 precise main &gt; /etc/apt/sources.list.d/gnome15.list"
    sudo apt-get update

<p>Finally install the packages based on your Desktop Enviroment:</p>

    sudo apt-get install gnome15-suite-gnome-shell
    sudo apt-get install gnome15-suite-gnome-fallback
    sudo apt-get install gnome15-suite-kde
    sudo apt-get install gnome15-suite-unity
    sudo apt-get install gnome15-suite-xfce
    sudo apt-get install gnome15-suite-other

<p>By default, the g15-direct and the g19-direct drivers are installed. Depending on the device you use, you may need to install another driver. See <a title="driver compatibility table " href="http://www.russo79.com/gnome15#driver-matrix" target="_blank">Driver Compatibility Table</a> for more information.</p>

    sudo apt-get install gnome15-driver-kernel
    sudo apt-get install gnome15-driver-g930

<p>I recommend (at time of writing) NOT to install the kernel driver. I had issues when I first insalled several months ago and have not as of yet tested again with the latest HWE kernel. But feel free to try.<br>
There is also a custom installation route if you wish to go down to choose plugins/drivers to install but I will not cover that here.<br>
Once the drivers are installed the G15 drivers and desktop service should start on login and you should see a nice new icon in your system tray for your tweaking pleasure.</p>
<p>Once again a big shout out to <a title="Russo79" href="http://www.russo79.com" target="_blank">Russo79</a> and the <a href="http://www.russo79.com/forum" target="_blank">forums</a> of his page. If you have any questions feel free to post here but I encourage you to visit Russo's site if you have one of the Logitech keyboards he supports. It's more than just the G510s if you were wondering.</p>
