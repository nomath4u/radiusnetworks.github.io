---
layout: android-ibeacon-library
---


<img src="images/ibeacon.png" style="display:block; float:left; width:30%"/>
<p style="height=20px"></p>

###Think iBeacons only work with Apple devices?  Not anymore.

With Radius Networks' Android iBeacon Service library, you can make Android versions of your iOS apps that use iBeacons, or come up with new Android-only creations.  Best of all, the library is available for FREE!

### About iBeacons

iBeacons are small hardware devices that send out Low Energy Bluetooth signals with unique identifiers.
These are useful for building mobile apps that can "see" the iBeacons, and approximate how far they are away, 
up to a hundred feet or so.
Apple came up with the technology as part of iOS7, which natively contains APIs to interact with them. 

### What does this library do?

It allows Android devices to use iBeacons much like iOS devices do.  An app can request to get notifications when one
or more iBeacons appear or disappear.  An app can also request to get a ranging update from one or more iBeacons
at a frequency of 1Hz.  The [iBeacon Locate App](https://play.google.com/store/apps/details?id=com.radiusnetworks.ibeaconlocate&hl=en) in the Google Play store demonstrates these capabilities.

### How do I get an iBeacon?

Radius Networks offers many options.

You can buy our [iBeacon Development Kit](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit.html) that includes everything you need to get started.  If you have a Mac, our [MacBeacon](http://www.radiusnetworks.com/macbeacon-app.html) application will turn your computer into an iBeacon.
Radius Networks also sells [active iBeacons](http://www.radiusnetworks.com/ibeacon.html) suitable for deployment.   For testing purposes, an iBeacon can be made out of an any iOS7 device that supports Low Energy Bluetooth using
Radius Netowrks' free [iBeacon Locate app ](https://itunes.apple.com/us/app/ibeacon-locate/id738709014). 
Unfortunately, it is not possible to make an iBeacon out of an Android device, because the Android Bluetooth LE APIs do not support the peripheral mode needed  to send advertisement packets like in iBeacon.  Fortunately, Radius
Radius Networks provides a free [Linux virtual machine](http://developer.radiusnetworks.com/ibeacon/virtual.html) that when paired with a cheap Bluetooth LE dongle, acts as an iBeacon.   

