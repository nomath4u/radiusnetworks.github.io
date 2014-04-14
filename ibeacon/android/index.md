---
layout: android-ibeacon-library
---


<img src="images/ibeacon.png" style="display:block; float:left; width:30%"/>
<p style="height:15px"></p>

###Think iBeacon technology only works with Apple devices?  Not anymore.

With Radius Networks' Android Beacon Service library, you can make Android versions of your iOS apps that use iBeacon technology, or come up with new Android-only creations.  Best of all, the library is available for FREE!

### About Proximity Beacons and iBeacon Technology

Proximity beacons are small hardware devices that send out Low Energy Bluetooth signals with unique identifiers.
These are useful for building mobile apps that can "see" the beacons, and approximate how far they are away,
up to a hundred feet or so.
Apple came up with the technology as part of iOS7, which natively contains APIs to interact with them.

### What Does This Library Do?

It allows Android devices to use iBeacon technology much like iOS devices do.  An app can request to get notifications when one
or more proximity beacons appear or disappear.  An app can also request to get a ranging update from one or more beacons
at a frequency of 1Hz.  The [iBeacon Locate App](https://play.google.com/store/apps/details?id=com.radiusnetworks.ibeaconlocate&hl=en) in the Google Play store demonstrates these capabilities.

### How Do I Get a Beacon?

Radius Networks offers many options.

You can buy our [Beacon Development Kit](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit.html) that includes everything you need to get started.  If you have a Mac, our [MacBeacon](http://www.radiusnetworks.com/macbeacon-app.html) application will turn your computer into a proximity beacon with iBeacon™ technology.
Radius Networks also sells [active beacons](http://www.radiusnetworks.com/ibeacon/buy-beacons.html) suitable for deployment.   For testing purposes, a beacon can be made out of an any iOS7 device that supports Low Energy Bluetooth using
Radius Networks' free [iBeacon Locate app ](https://itunes.apple.com/us/app/ibeacon-locate/id738709014).
Unfortunately, it is not possible to make an proximity beacon out of an Android device, because the Android Bluetooth LE APIs do not support the peripheral mode needed  to send advertisement packets.  Fortunately, Radius
Radius Networks provides a free [Linux virtual machine](http://developer.radiusnetworks.com/ibeacon/virtual.html) that when paired with a cheap Bluetooth LE dongle, acts as a proximity beacon.

