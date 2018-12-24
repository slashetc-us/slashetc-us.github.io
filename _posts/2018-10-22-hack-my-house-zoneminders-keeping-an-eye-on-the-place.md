---
layout: post
title: 'Hack My House: ZoneMinder’s Keeping an Eye on the Place'
date: 2018-10-22 14:00:13.000000000 -04:00
author: TT-RSS Published Article
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- TT-RSS Published Articles
tags: []
meta:
  _wpas_done_all: '1'
---
<div>
<p>Hacks are often born out of unfortunate circumstances. My unfortunate circumstance was a robbery&ndash; the back door of the remodel was kicked in, and a generator was carted off. Once the police report was filed and the door screwed shut, it was time to order cameras. Oh, and record the models and serial numbers of all my tools.</p>
<p><img src="{{ site.baseurl }}/assets/img_20181017_175522-e1539843085631.jpg?w=400&amp;h=341" alt="" /></p>
<p>We&rsquo;re going to use Power over Ethernet (POE) network cameras and a ZoneMinder install. ZoneMinder has a network trigger capability, and we&rsquo;ll wire some magnetic switches to <a href="https://hackaday.com/2018/10/08/hack-my-house-running-raspberry-pi-without-an-sd-card/" rel="noopener noreferrer" target="_blank">our network of PXE booting Pis</a>, using those to inform the Zoneminder server of door opening events. Beyond that, many newer cameras support the Open Network Video Interface Forum (ONVIF) protocol and can do onboard motion detection. We&rsquo;ll use the same script, running on the Pi, to forward those events as well.</p>
<p>Many of you have pointed out that Zoneminder isn&rsquo;t the only option for open source camera management. MotionEyeOS,&nbsp;Pikrellcam, and Shinobi are all valid options.&nbsp; I&rsquo;m most familiar with Zoneminder, even <a href="http://twit.tv/shows/floss-weekly/episodes/496" target="_blank" rel="noopener noreferrer">interviewing them on FLOSS Weekly</a>, so that&rsquo;s what I&rsquo;m using.&nbsp; Perhaps at some point we can revisit this decision, and compare the existing video surveillance systems.</p>
<p><span></span></p>
<h2>Cameras and Installation</h2>
<p>Zoneminder generally works with any camera that follows the modern standards.&nbsp; I&rsquo;m using a handful of four-megapixel Trendnet&nbsp; TV-IP314PI cameras, which seem to be the sweet spot for cost and quality. These cameras have a particularly odd quirk that took me several days to understand. To get motion detection running on the camera itself, I needed to update the camera firmware, but the browser interface simply refused to select a firmware file to upload. Many IP cameras make use of a browser plugin to view the live stream, and older firmware on the Trendnet units also require that plugin in order to upload the firmware update. I finally turned to a Windows 7 VM, installed the browser plugin, and got the firmware updated.</p>
<p>Camera placement takes planning to be effective. Coverage of the front and back doors is a must, and seeing whether your garage door is open is quite useful as well. I also decided to cover all the windows. If you can manage, it&rsquo;s useful to stretch the Ethernet cable around the outside of the house, and hold the camera in place while you or a buddy pulls it up on a cell phone, in order to find the best placement. All told, I hung nine cameras.</p>
<h2>Ethernet in Living Color</h2>
<figure><img src="{{ site.baseurl }}/assets/img_20181017_175746.jpg?w=300&amp;h=400" alt="" /><br />
<figcaption>Ethernet, lots of Ethernet.</figcaption>
</figure>
<p>If you&rsquo;re keeping track, that&rsquo;s a grand total of <em>many</em> Ethernet cables. To help when sorting them out, I bought several colors of cabling: red, green, yellow, blue, and white. I highly recommend a color code. Mine goes like this: PoE cameras use yellow Ethernet, Raspberry Pis use red. Each room of the house gets two more cables, white for a VoIP phone and blue for a generic connection to the network. The green cable is for running something other than Ethernet over Cat 5, like a door or temperature sensor.</p>
<p>When I finish the interior, I&rsquo;ll terminate these Ethernet cables on a set of patch panels, using color matching keystone jacks. The colors are more than just a novelty&ndash; when you&rsquo;ve run a bunch of cables, it&rsquo;s far too easy to lose track of which one is which.</p>
<p>Some ports need PoE and some don&rsquo;t. I&rsquo;m also planning to use VLANs to separate the various networks. Once it&rsquo;s all done, we can take a deeper dive into selecting and configuring the smart switches that run the house. Keeping the Ethernet cabling as neat as possible will make the eventual configuration task much more manageable.</p>
<h2>Zoneminder</h2>
<p>Zoneminder has some <a href="https://zoneminder.readthedocs.io/en/stable/installationguide/" target="_blank" rel="noopener noreferrer">great documentation</a>&nbsp;on how to get an install up and running, so rather than cover that ground again, we&rsquo;ll look closer at how to use the Raspberry Pi network to link door sensors and ONVIF motion detection into the loop. To this end, I&rsquo;ve put together the <code>zmhelper</code> service, <a href="http://github.com/jp-bennett/zmhelper" target="_blank" rel="noopener noreferrer">and made the code available on GitHub</a>. Well look at the more interesting snippets below.</p>
<p><code>Zmtrigger</code> is the Zoneminder component we&rsquo;ll be talking to, and it listens on TCP port 6802. On the Zoneminder server, we&rsquo;ll need to open that port in the firewall, and then enable the&nbsp;<code>OPT_TRIGGERS</code> option through the web interface. This allows our service, running on a Pi, to inform Zoneminder that something important happened, and that it should start recording. Zoneminder can do motion detection natively, but offloading that work can be helpful when trying to run multiple cameras.</p>
<h2>Speaking GPIO</h2>
<p>A door sensor is just a simple magnetic switch installed into the door jamb. When the door is closed, the magnet in the door pulls the switch closed, completing the circuit. The Pi&rsquo;s GPIO ports are perfect to monitor this. One wire from the sensor goes to ground, the other to the GPIO pin. It&rsquo;s best to put a resistor between that pin and the switch, to protect in the case of accidentally powering it. Some of the GPIO pins go high or low during the boot process, and shorting a hot GPIO directly to ground is a BAD THING(tm). Some of you might be thinking about pull-up resistors. The Raspberry Pi has software configurable pull-up and pull-down resistors that are already built in, so we don&rsquo;t need the external resistors.</p>
<p>GPIO event detection isn&rsquo;t limited to just door sensors. Motion detectors are the other interesting possibility. I hope to eventually look at this in more detail, in the context of retooling an old security system to be powered by a Pi.</p>
<p>Rather than poll that GPIO pin every few seconds, we set up an interrupt handler.&nbsp; This allows the event reporting code to trigger more quickly, and allows us to watch for ONVIF events at the same time. The other bit of magic going on here is the debouncing logic. The GPIO library does well at filtering out the bounces when there is a legitimate trigger, like the door opening. It fails at filtering out the bounces generated by the switch closing. To overcome this, we sleep for the bouncetime and check the GPIO status again.</p>
<pre>
  GPIO.setmode(GPIO.BOARD)
  GPIO.setup(gpio_pinnum, GPIO.IN, pull_up_down=gpio_resistor)
  def handler(pin):
      time.sleep(gpio_bouncetime/1000)
      if GPIO.input(gpio_pinnum) == gpio_active_state: #Debounce check.  If we're still active, it's a real event.
          s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
          s.connect((zmip, zmport))
          s.send(gpio_mid +'|on+20|' + gpio_escore + '|' + gpio_ecause + '|' + gpio_etext)
          s.close()
          print("DOOR opened!")
  GPIO.add_event_detect(gpio_pinnum, gpio_edge, handler, bouncetime=gpio_bouncetime)
</pre>
<h2>ONVIF Events</h2>
<p>If ONVIF is configured, then we connect to the camera, pull any events in the cue, and wait for new events. The code here is blocking&ndash; it stops and waits for the response from the camera. That response is intentionally delayed until a new event is ready. The code then scans through the event data looking for whether there was motion detected.</p>
<pre>
  mycam = ONVIFCamera(camIP, 80, username, password, wsdl_dir='/home/pi/.local/wsdl/')
  event_service = mycam.create_events_service()
  pullpoint = mycam.create_pullpoint_service()
  req = pullpoint.create_type('PullMessages')
  req.MessageLimit=100
  while True:
    messages = Client.dict(pullpoint.PullMessages(req))
    if 'NotificationMessage' in messages:
      try:
        messages = messages['NotificationMessage']
        for x in messages:
          message = Client.dict(Client.dict(Client.dict(Client.dict(Client.dict(x)['Message'])['Message'])['Data'])['SimpleItem'])
          if message['_Name'] == 'IsMotion' and message['_Value'] == 'true':
            if time.time() - last_trigger &gt; 15:
              print("Triggering!")
              last_trigger = time.time()
              s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
              s.connect((zmip, zmport))
              s.send(onvif_mid +'|on+20|' + onvif_escore+ '|' + onvif_ecause + '|' + onvif_etext)
              s.close()
            break
      except:
        print('Error fetching event')
</pre>
<p>We&rsquo;ve looked at our first real use of the Raspberry Pi, feeding events into a Zoneminder instance. There&rsquo;s more to come, like breaking the proprietary protocol on a garage door opener, building a touchscreen thermostat, and looking at how to securely access everything remotely. As always, sound off below about what you want to see in the future. Until next time, Happy Hacking!</p>
<p><img src="{{ site.baseurl }}/assets/M4MhaK2XHjI" alt="" /></div>
<p>Read More <a href="https://hackaday.com/2018/10/23/hack-my-house-zoneminders-keeping-an-eye-on-the-place/" target="_blank">here</a>.</p>
