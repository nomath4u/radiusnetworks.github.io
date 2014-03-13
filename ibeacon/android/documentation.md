---
layout: android-ibeacon-library
---

### JavaDocs

The full JavaDocs of the open-source APIs are available [here.](http://developer.radiusnetworks.com/android-ibeacon-service/doc/)

The full JavaDocs of the Pro library APIs are available 
[here.](/ibeacon/android/pro/javadocs/).  Additional documentation for the pro library is [here](/ibeacon/android/pro/documentation.html).



### iOS API Mapping

The API is a modeled of the iBeacon parts of the iOS Location SDK as much as possible. Key differences include:

* The Android library supports wildcards for *ALL* iBeacon identifiers, allowing you to look see any iBeacon.
* The Ranging updates come every 1.1 seconds instead of 1.0 seconds due to time synchronization issues with Android Bluetooth scanning.
* The distance estimating algorithm approximates the iOS implementation, but it is not identical
* Ranging updates may be passed to background applications as well as foreground applications.

The table below shows the Android Class and the equivalent iOS class, with a link to the documentation.

Android | iOS 
------- | --- 
[Region](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/Region.html)  | [CLBeaconRegion](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLBeaconRegion_class/Reference/Reference.html)
[IBeacon](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/IBeacon.html)  | [CLBeacon](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLBeacon_class/Reference/Reference.html)
[IBeaconManager](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/IBeaconManager.html)  | [CLLocationManager](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html)
[IBeaconConsumer](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/IBeaconConsumer.html)  | N/A 
[MonitorNotifier](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/MonitorNotifier.html)  | [CLLocationManagerDelegate](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManagerDelegate_Protocol/CLLocationManagerDelegate/CLLocationManagerDelegate.html)
[RangeNotifier](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/RangeNotifier.html)  | [CLLocationManagerDelegate](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManagerDelegate_Protocol/CLLocationManagerDelegate/CLLocationManagerDelegate.html)

### Supported Platforms

This library requires Android 4.3 (SDK version 18), because that is earliest SDK that supports low energy Bluetooth. 
The mobile device must also have a low energy Bluetooth chipset, sometimes called BLE or Bluetooth 4.0.  Most mid-range and above Android devices that started manufacturing in Mid-2012 or later have Bluetooth LE.

As of December 2013, Android devices known to have BLE include: Samsung Galaxy S3/S4, Samsung Galaxy Note II, Sansung Galaxy Tab 3 7.0/8.1/10.0, Nexus 7 2013 edition, Nexus 4/5, HTC One HTC Butterfly, HTC Evo LTE, HTC Desire X, LG G Pad, LG Optimus G, LG G2, Sony Experia Tablet S/Z, Sony Experia Z/L/P/M, Motorola Droid DNA, Motorola Droid Razr I/HD/Maxx, Motorola Moto X, Huawei Ascend G610/G700/G740/Y511/P2/D2/Mate, Huawei A199

Note:  The Kindle Fire HD supports Bluetooth 4.0, but there are currently no plans to make it upgradable to Android 4.3 without rooting the device.

### Known issues:

* [On the Nexus 4 and the Moto G, Bluetooth and WiFi do not work properly at the same time.](https://code.google.com/p/android/issues/detail?id=41631)  If this library is used on the Nexus 4 when WiFi is active, it may cause a dropped WiFi connection, inability to do Wifi scans, delays in seeing iBeacons, or a total inability to see iBeacons.  Turning off WiFi on the Nexus 4 solves these problems.  Similar problems have been reported, but uncomfirmed, with Nexus 7 devices

### License

This software is available under the Apache License 2.0, allowing you to use the library in your applications.
