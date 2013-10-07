---
layout: blog 
---

#Reverse Engineering 
#the iBeacon Profile

October 1, 2013

When Apple announced iBeacons at WWDC back in June, many of us developers were excited about them.  Apple made a demo program available, <a href='https://developer.apple.com/downloads/index.action?name=WWDC%202013'>AirLocate</a>, which could make an iPhone or iPad act as an iBeacon for development purposes.  But there were lots of problems with using this for development.

##The Problems

First, many of us don't have a pile of extra iOS devices lying around -- especially the latest and greatest ones with the Bluetooth LE chipset (iPhone 4s+, iPad 3rd generation+) needed to run AirLocate.  Unless you have two such devices (one to act as an iBeacon and the other to host your app under development), you have to buy another one for $600+.  Second, you have to be a member of the iOS Developer Program to get Air Locate on your iOS devices.  That usually isn't the case for Android developers.  Finally, what if you want to build an iBeacon on your own hardware?  Apple so far hasn't told us how to do that.

The first step to solving these problems is to figure out how iBeacons work.  That means it's time to do a little reverse engineering.

##The Reverse Engineering Procedure

Knowing that iBeacons use Bluetooth LE, we figured you could use a bluetooth sniffer to figure out what is going on.  Texas Instruments sells a Bluetooth LE designed for development tasks like sniffing, so we ordered one to see what we could do.

Parts:

* iPad 3rd generation
* Apple's AirLocate program, installed on the iPad
* Windows computer
* Texas Instruments CC2540 Bluetooth LE dongle, inserted into the Windows computer
* [TI SmartRF Packet Sniffer tool](www.ti.com/tool/packet-sniffer), loaded on the Windows computer

We started up the packet sniffer tool, selected "Bluetooth LE" as the protocol, and started a capture.  Not much was going on.  Then we fired up AirLocate, configured it to act as an iBeacon using AirLocate's default proximityUUID E2C56DB5-DFFB-48D2-B060-D0F5A71096E0, major 0 and minor 0.  As soon as we did this, we started seeing new bluetooth packets once per second.

The results on the screen were encouraging.  It showed the raw hex bytes in the captured packets, and it was obvious that they included the proximityUUID.  But we needed to export the a full capture file to see if this was consistently true.

##The Results

The packet sniffer tool let us output its capture in a [binary format](http://e2e.ti.com/support/low_power_rf/f/155/t/240865.aspx) that I had to write Python script to parse.  Once I did, I could see the contents of the individual packets as hex bytes:

```
 0      1            0us (+   0.000ms) [ 47]
d6 be 89 8e 40 24 5 a2 17 6e 3d 71 2 1 1a 1a ff 4c 0 2 15 e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 0 0 0 0 c5 52 ab 8d 38 a5
 0      2        31252us (+  31.252ms) [ 47]
d6 be 89 8e 40 24 5 a2 17 6e 3d 71 2 1 1a 1a ff 4c 0 2 15 e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 0 0 0 0 c5 52 ab 8d 38 a5
 0      3        66253us (+  35.001ms) [ 47]
d6 be 89 8e 40 24 5 a2 17 6e 3d 71 2 1 1a 1a ff 4c 0 2 15 e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 0 0 0 0 c5 52 ab 8d 38 a5
 0      4       100004us (+  33.751ms) [ 47]
d6 be 89 8e 40 24 5 a2 17 6e 3d 71 2 1 1a 1a ff 4c 0 2 15 e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 0 0 0 0 c5 52 ab 8d 38 a5
 0      5       132502us (+  32.498ms) [ 47]
d6 be 89 8e 40 24 5 a2 17 6e 3d 71 2 1 1a 1a ff 4c 0 2 15 e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 0 0 0 0 c5 52 ab 8d 38 a5
 0      6       167505us (+  35.003ms) [ 47]
d6 be 89 8e 40 24 5 a2 17 6e 3d 71 2 1 1a 1a ff 4c 0 2 15 e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 0 0 0 0 c5 52 ab 8d 38 a5

```

As you can see from the above, the same packet is repeated over and over again.  And this packet contains the profile UUID of the iBeacon (E2C56DB5-DFFB-48D2-B060-D0F5A71096E0)

##Conclusions

But what do these bytes mean?  [TI's wiki pages helped a bit](http://processors.wiki.ti.com/index.php/BLE_sniffer_guide#Advertisement_packets) as did this page from the [EE Times](http://www.eetimes.com/document.asp?doc_id=1278927)  Based on the information here, I figured out the following about the packet structure:

```
# Packet Header Starts Here
d6 be 89 8e # Access address for advertising data (this is always the same fixed value)
40 # Advertising Channel PDU Header byte 0.  Contains: (type = 0), (tx add = 1), (rx add = 0)
24 # Advertising Channel PDU Header byte 1.  Contains:  (length = total bytes of the advertising payload + 6 bytes for the mac address.)
05 a2 17 6e 3d 71 # Mac address.... note this does not seem to be the actual bluetooth mac of the iPad -- apparently it gets spoofed by iOS
# Actual Advertising Data Starts Here
02 01 1a 1a ff 4c 00 02 15 # Apple's static prefix to the advertising data -- this is always the same
e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 # iBeacon profileUUID
00 00 # major (LSB first)  
00 00 # minor (LSB first)
c5 # The 2's complement of the calibrated Tx Power
# Actual Advertising Data Ends Here
52 ab 8d 38 a5 # CRC
```

To validate that this was what was going on, we tried changing different values of major, minor and the profileUUID, and they changed exactly as we expected.

Success!  We had reverse engineered the iBeacon bluetooth profile.
