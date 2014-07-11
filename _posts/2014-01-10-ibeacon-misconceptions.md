---
layout: post
title: 5 fundamental misconceptions about iBeacon™ technology
author: Christopher Sexton
---

Beacons are great. A rock solid way to build proximity solutions into mobile applications. However there is some confusion about how they actually work. Here are the 5 most common misconceptions about iBeacon™ technology that we see.

# #1 Beacons Deliver Content

They don't deliver anything. They simply broadcast a few identifiers. Beacons broadcast a UUID, Major Value, and Minor Value. No user consumable content is broadcast, just these IDs.

# #2 Beacons Know When They Are Detected

They don't know anything. They simply broadcast. There is no pairing, and no exchange of data from your device to the beacon.

# #3 Beacons Are Detected Immediately

The fact of the matter is beacons are not immediately noticed all the time. Sometimes they are noticed quite quickly, but sometimes it can be [5 minutes or more](/2013/11/13/ibeacon-monitoring-in-the-background-and-foreground.html) before a phone responds to an beacon. This is due to a number of factors including:

1. How frequently a beacon broadcasts. That's right, the BLE radio has to announce it's identifiers. This is not a constant-on type thing. How frequently this happens is up to the folks that built the beacon. They could broadcast 10 times a second, or once a minute. This is especially a concern with battery powered beacons that are worried about a very long battery life.

2. How often the phone scans for a beacon. The trick here is that there needs to be an overlap from the broadcast and the scan. And just like the beacon manufacturer this can depend on the phone hardware. The newer iPhones have improved support for BLE, and our experimentation shows they respond faster.

# #4 Beacon Distance is Accurate

If you speak with the Apple engineers explaining about beacons, you will notice they never say "distance" without first saying "estimated". This is because the distance is really just that, an estimate. The way the distance is estimated is with the RSSI, or the signal strength of the broadcast.

But this is not to say that the distance estimate is not useful. It can be, the closer you are the more accurate, and it can be calibrated.

# #5 Beacons can only be Detected by Apple Devices

Since Apple came up with the concept, it is understandable that people think beacons would limited to their hardware. Luckily an beacon just broadcasts a standard Bluetooth Low Energy signal, and any device that supports BLE should be able to detect it.

iOS has this capability built into CoreLocation. BLE enabled Android devices can take advantage of Radius' free and open source [Android iBeacon Library](http://developer.radiusnetworks.com/ibeacon/android/).

## Building Blocks for a Bigger Solution

Beacons are an exciting emerging technology, but they are just one critical building block in a complete proximity-enabled solution. Radius Networks is working hard to be your best choice for all the parts and pieces it takes to help developers take advantage of all the benefits that beacons and proximity can provide.


