---
layout: blog 
---
# How to Make an iBeacon Out of a Raspberry Pi

By David G. Young

October 8, 2013

Want a standalone iBeacon to use for development?  Here's a procedure you can use to build your own for under $100.  If you don't want to go to the trouble, we can ship you a pre-built development kit including everything below (except the cell phone charger -- you probably have one already!)

iBeacon Parts list:

1. [A Raspberry Pi computer board](http://www.raspberrypi.org/) (Model A or Model B)
2. A 4GB or larger [SD card](http://www.amazon.com/SanDisk-Class-Flash-Memory-SDSDB-008G-AFFP/dp/B007JRB0TC)
3. [A Bluetooth LE dongle.](http://www.amazon.com/dp/B007GFX0PY/ref=pe_385040_30332190_pe_175190_21431760_M3T1_ST1_dp_1)
4. A standard miro-USB 5V cell phone charger

Tools you will need:

1. A USB Keyboard
2. A USB Mouse
3. A computer monitor with HDMI input
4. An HDMI cable
5. A network cable for internet access

Step 1.  Set up your Raspberry Pi

The Raspberry Pi is a nifty little computer board that is great for hobbyists.  In order to make it into an iBeacon you'll have to go through its basic setup, hooking it up to a keyboard, mouse and monitor, and plugging it into your router for internet access.  The biggest part of the setup involves preparing the SD card with its operating system.  To do this, follow [this guide](http://www.raspberrypi.org/wp-content/uploads/2012/04/quick-start-guide-v2_1.pdf) and select the "raspbian" operating system option during install.

Step 2. Unplug the mouse, and plug in the keyboard, Bluetooth LE dongle, monitor, and ethernet cable

Step 3. Log in to your Raspberry Pi

Using the keyboard, log in using the credentials, username: pi, password: raspberry

Step 4. Verify you have internet access:

```
$ ping www.google.com
PING www.google.com (74.125.228.116): 56 data bytes
64 bytes from 74.125.228.116: icmp_seq=0 ttl=55 time=34.267 ms
64 bytes from 74.125.228.116: icmp_seq=1 ttl=55 time=30.908 ms
```

Step 5. Install Linux build tools
