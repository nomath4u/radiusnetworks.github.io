---
layout: blog 
---
#Android iBeacon Library Now Available

By David G. Young

October 7, 2013

The release of iOS 7 last month has made it possible to develop some pretty cool proximity-aware apps for iPhone and iPad devices.
But what about Android?  With our new Android iBeacon Library, you can now build the same cool proximity-aware apps on the world's
largest mobile operating system.  We've released it under the Apache 2 license, which means you can embed it in your own applications
and sell it on the Google Play store.

## How it Works

An iBeacon works by advertising Bluetooth Low Energy packets once per second.  It sends out a three part unique identifier that can be
received by your app in real time.  When your app sees the identifier, it has a pretty good idea where it is.  iBeacons have a range of only 
a few hundred feet, and you can get an estimate of the actual distance to the iBeacon by analyzing the signal strength.  In iOS 7, built-in
APIs perform all these functions for you.  On Android, you can get very similar APIs by using our library.  On both platforms, the APIs perform
two basic functions:

1. Monitoring - The Monitoring API allows your app to look for one or more iBeacons and get notified when they come into range or go out of range.
2. Ranging - The Ranging API allows your app to get updates once per second on the estimated distance to the iBeacons that it sees.

## Device Support

This library is possible because the July release of Android 4.3 brought OS-level support for Bluetooth LE.  Not many Android devices have
received that update so far (although my Nexus 4 has), but carriers are expected to roll it out to many higher-end phones like the Galaxy S3/S4 in
coming months.  In addition to Android 4.3 (SDK version 18), devices also have to have a low energy Bluetooth chipset, sometimes called BLE or Bluetooth 4.0. As of September 2013, Android devices known to have BLE include: Samsung Galaxy S3/S4, Samsung Galaxy Note II, HTC One, Nexus 7 2013 edition, Nexus 4, HTC Butterfly, Droid DNA.

## Capabilities not in iOS

The library iOS 7 iBeacon APIs as much as possible.  But because Android is a more open platform, you can do some cool things with it
that you can't on iOS.  For example, you can monitor or range *any* iBeacon in the world, even if you don't know its unique identifier.  You can also ranging in the background, and have your app come to life when you get a specific distance from an iBeacon.

## Getting Started

If you want to get started using the library, check out the developer documentation in the [github project](https://github.com/RadiusNetworks/android-ibeacon-service).  Before long, you'll need to get your hands on an iBeacon.  If you have a Bluetooth LE-capable iOS device, you can use [Apple's AirLocate sample program](https://developer.apple.com/downloads/index.action?name=WWDC%202013).  We also provide a free [iBeacon Virtual machine](https://github.com/RadiusNetworks/android-ibeacon-service/wiki/Virtual-iBeacon), which requires only a Bluetooth LE dongle on  your computer.  If you want a standalone iBeacon, you can even make one out of a Raspberry Pi.  That's the subject of my next blog post.


[See all blog posts](http://developer.radiusnetworks.com/blog)
