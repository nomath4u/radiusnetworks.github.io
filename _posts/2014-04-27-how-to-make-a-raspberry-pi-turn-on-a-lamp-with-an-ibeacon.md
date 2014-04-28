---
layout: post
title: How To Make a Raspberry Pi Turn on a Lamp with an iBeacon
author: James Nebeker
---

iBeacon proximity technology is an exciting tool already being used by retailers and developers to build location-aware apps to improve user experience and much more.  In this post, we’ll show you an easy way to harness iBeacon technology in your own home, without having to do any serious programming or developing.  In this simple example, we’ll be using a Raspberry Pi to turn on a light whenever it detects a specific beacon.

<img style="margin:10px; height: 200px; border: thin solid #333;float:right;" src='/img/pibeacon.jpg'>


###Parts list:

1. [Beacon Development Kit](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit.html) (alternately, you can supply your own Raspberry Pi, bluetooth LE dongle, and SD card)
2. Standard micro-USB 5V cell phone charger
3. [PowerSwitch Tail II](http://www.amazon.com/POWERSWITCHTAIL-COM-PowerSwitch-Tail-II/dp/B00B888VHM/ref=sr_sp-atf_title_1_1?ie=UTF8&qid=1398462203&sr=8-1&keywords=powerswitch+tail)
4. [Jumper wires](http://www.robotmesh.com/jumper-wires-7-8-f-m-10-pack) (recommended) or household wire
5. Desk lamp or other light source

###Other tools you will need:

1. [Proximity Beacon](http://www.radiusnetworks.com/ibeacon/buy-beacons.html) (compatible with iBeacon technology)
2. USB Keyboard
3. Computer monitor with HDMI input
4. HDMI cable
5. Small flat-head screwdriver
6. Network cable for internet access (optional)

Before we put this all together let’s go over each part individually, for starters we’ll assume you have your Raspberry Pi already set up (with BlueZ, the Linux bluetooth stack, installed) and have some familiarity with basic Linux command line operations.  If not, or if you want a quick introduction to the Raspberry Pi and iBeacons, check out our blog post, [How to Make an iBeacon Out of a Raspberry Pi](http://developer.radiusnetworks.com/2013/10/09/how-to-make-an-ibeacon-out-of-a-raspberry-pi.html), to get acquainted with Linux,  the Raspberry Pi, and iBeacon technology.  

## Scanning for beacons

The ability to scan for nearby proximity beacons is the latest feature of the Beacon Development Kit, new to [Version 3.0](http://developer.radiusnetworks.com/2014/04/27/ibeacon-development-kit-version-3.html).  In this guide we will be using the newest development kit software to do the scanning.  Information on how to download this update can be found [here](http://developer.radiusnetworks.com/ibeacon/beacon-dev-kit-update.html).  If you already have your own Raspberry Pi and bluetooth dongle you can create your own iBeacon scan script using the steps I’ve outlined in [this post](http://stackoverflow.com/a/21790504/1461050).  

Before we can start scanning we need to make sure we have a beacon nearby to test with.  The easiest way to do this is probably to download our free [Locate for iBeacon](https://itunes.apple.com/us/app/locate-for-ibeacon/id738709014?mt=8) app for iOS.  You can also check out our [products page](http://www.radiusnetworks.com/ibeacon/buy-beacons.html) for other beacons to test with.  For this tutorial, I will be using a test beacon with the following identifiers:

* UUID: 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 
* MAJOR: 1 
* MINOR: 1

Once you have your test beacon up and running, let’s do a quick test scan.  First, make sure the development kit isn’t currently broadcasting using the `ibeacon stop` command, then run the following command to start a scan and you should see the identifiers of your test beacon.

```
$ ibeacon scan
UUID: 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 MAJOR: 1 MINOR: 1 POWER: -59
UUID: 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 MAJOR: 1 MINOR: 1 POWER: -59
UUID: 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 MAJOR: 1 MINOR: 1 POWER: -59
```

To exit a scan press control-c. 

<div style="font-weight: bold;">Note:</div> You may experience some instability when using the new scan feature for extended periods of time in high-traffic areas due to an error where the bluetooth adapter enters a bad state.  The only way to recover from this state is to power cycle the adapter or reboot the machine.  We are continuing to work to minimize this issue and will be providing additional updates.    

For triggering the light in this example, we’ll be using the `-b` option for bare output.  Go ahead and try it out, and you should see something like this:

```
$ ibeacon scan -b
2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 1 1 -59
2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 1 1 -59
2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 1 1 -59
```

Now that we can detect nearby beacons with the Raspberry Pi, the next step is turning the light on and off.

## Controlling the light

To control the light we will be taking advantage of the GPIO pins on the Raspberry Pi, which can supply a voltage when commanded, and the PowerSwitch Tail II, which switches power from a wall socket with a small control voltage.  The first step is to connect the power switch to the GPIO pins on your Raspberry Pi, we recommend using [jumper wires](http://www.robotmesh.com/jumper-wires-7-8-f-m-10-pack) to make things easier.  The diagram below shows the wiring configuration, the ground pin on the Raspberry Pi is attached to the negative input on the switch and the GPIO1 pin on the Pi is attached to the positive input on the switch.  Use a small flat-head screwdriver to secure the wires into the two slots on the power switch.

<img style="margin:10px; height: 400px; float:middle;" src='http://i.imgur.com/3bHlrVG.png'>

Once everything is connected, go ahead and plug the power switch into the wall and plug your light into the switch.  Make sure any switches on the light itself are in the on position.  Now we’re ready to test the power switch, to control the voltage on the GPIO pins we’ll be using the [Wiring Pi](https://projects.drogon.net/raspberry-pi/wiringpi/) library which is installed by default on the Beacon Development Kit.  The first step is to change GPIO 1 into `OUT` mode.  Then command the pin to turn on, which will activate the power switch and turn the light on.  To do this run the following commands (shouting “Let there be light!” is optional):

```
$ gpio mode 1 out
$ gpio write 1 1 
```

You can turn the light off by switching GPIO 1 off:

```
$ gpio write 1 0
```

Now all that’s left is to combine this with beacon scanning and we’ll have an automated iBeacon lightswitch.  
 
## Putting it all together

We’re going to write a little bash script that will read the input from a beacon scan line by line and search for a specific beacon’s identifiers.  Once it detects the beacon it will switch on the light, if you exit the script with control-C the light will be switched off.  Use your favorite text editor (e.g., vi, nano) to create the `welcome_light` script seen below:

```
$ nano welcome_light
```

Paste in the contents of this block:

```
#!/bin/bash

gpio mode 1 out
trap "gpio write 1 0" exit
while read line
do
  if [[ `echo $line | grep "2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 1 1" ` ]]; then
    gpio write 1 1
  fi
done
```

Be sure to change the iBeacon identifiers in the if statement to the ones you'll be broadcasting with your test beacon. Save this file and get your beacon ready, make sure it is off for to start the test.  Now run the following command to start the scan and light switch sequence, it starts a beacon scan with the bare output option, piping its output into our `welcome_light` script.  If you made your own scan script, just run that script and pipe its output to `welcome_light`.

```
$ ibeacon scan -b | ./welcome_light
```

Once the scan is started, activate your beacon and you should see the light turn on automatically.  Check out the video clip below to see the demo in action.

<iframe width="420" height="345"
src="https://www.youtube.com/watch?v=hJ3OjPlCtsc">
</iframe>

For a homework assignment, you can build on this script to turn off the light when it no longer detects the beacon (i.e., when you leave the room).  	

## That’s it!

We hope this quick tutorial gave you a good example of the power of proximity beacon technology.  There are tons of other cool ways to take use beacons with a Raspberry Pi, we look forward to seeing what everyone can come up with.  

