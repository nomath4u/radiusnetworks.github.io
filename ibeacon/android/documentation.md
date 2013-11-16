---
layout: android-ibeacon-library
---

### JavaDocs

The full JavaDocs of the APIs are available [here.](http://developer.radiusnetworks.com/android-ibeacon-service/doc/)

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

## Known issues:

* [On the Nexus 4, Bluetooth and WiFi do not work properly at the same time.](https://code.google.com/p/android/issues/detail?id=41631)  If this library is used on the Nexus 4 when WiFi is active, it may cause a dropped WiFi connection, inability to do Wifi scans, delays in seeing iBeacons, or a total inability to see iBeacons.  Turning off WiFi on the Nexus 4 solves these problems.

When the service is running and scanning for Bluetooth devices, Wifi scans are blocked until the bluetooth scan stops.  Similarly, if a Wifi scan is started, bluetooth scans are blocked (along with discovery of iBeacons) until the Wifi scan completes.

## License

This software is available under the Apache License 2.0, allowing you to use the library in your applications.
