---
layout: post
title: 5 fundamental misconceptions about iBeacons
author: Christopher Sexton
---

iBeacons are great. A rock solid way to build proximity solutions into mobile applications. However there is some confusion about how they actually work. Here are the 5 most common misconceptions about iBeacons that we see.

# #1 iBeacons Deliver Content

They don't deliver anything. They simply broadcast a few identifiers. iBeacons broadcast a UUID, Major Value, and Minor Value. No user consumable content is broadcast, just these IDs.

# #2 iBeacons Know When They Are Detected

They don't know anything. They simply broadcast. There is no pairing, and no exchange of data from your device to the iBeacon.

# #3 iBeacons Are Detected Immediately

The fact of the matter is iBeacons are not immediately noticed all the time. Sometimes they are noticed quite quickly, but sometimes it can be [5 minutes or more](2013/11/13/ibeacon-monitoring-in-the-background-and-foreground.html) before a phone responds to an iBeacon. This is due to a number of factors including:

1. How frequently a beacon broadcasts. That's right, the BTE radio has to announce it's identifiers. This is not a constant-on type thing. How frequently this happens is up to the folks that built the beacon. They could broadcast 10 times a second, or once a minute. This is especially a concern with battery powered iBeacons that are worried about a very long battery life.

2. How often the phone scans for a beacon. The trick here is that there needs to be an overlap from the broadcast and the scan. And just like the iBeacon manufacturer this can depend on the phone hardware. The newer iPhones have improved support for BLE, and our experimentation shows they respond faster.

# #4 iBeacon Distance is Accurate

If speak with the Apple engineers explaining about iBeacons, you will notice they never say "distance" without first saying "estimated". This is because the distance is really just that, an estimate. The way the distance is estimated is with the RSSI, or the signal strength of the broadcast.

But this is not to say that the distance estimate is not useful. It can be, the closer you are the more accurate, and it can be calibrated.

# #5 iBeacons can be Automatically Updated

Now this depends on the iBeacon, but for most off-the-shelf iBeacons there is no way to update in the field. Well, aside from getting physical access to the device and manually updating the information. Normally the expectation is that the iBeacon would have some way to check for updates with a server.

Sure, iBeacon software running on a computer could do this pretty easily, but when you say iBeacon most people think that little button cell powered embedded device. They just can't be updated easily.

## Building Blocks for a Bigger Solution

All these limitations can be worked around. It normally involves building out a more sophisticated system. Radius Networks is working to build tools to help [with that](http://www.radiusnetworks.com/ibeacon-services.html).


