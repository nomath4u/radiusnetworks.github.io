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

<table>
  <tr>
    <td>Android</td>
    <td>iOS</td>
  </tr>
  <tr>
    <td>[Region](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/Region.html)</td>
    <td>[CLBeaconRegion](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLBeaconRegion_class/Reference/Reference.html)</td>
  </tr>
  <tr>
    <td>[IBeacon](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/IBeacon.html)</td>
    <td>[CLBeacon](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLBeacon_class/Reference/Reference.html)</td>
  </tr>
  <tr>
    <td>[IBeaconManager](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/IBeaconManager.html)</td>
    <td>[CLLocationManager](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html)</td>
  </tr>
  <tr>
    <td>[IBeaconConsumer](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/IBeaconConsumer.html)</td>
    <td>N/A</td>
  </tr>
  <tr>
    <td>[MonitorNotifier](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/MonitorNotifier.html)</td>
    <td>[CLLocationManagerDelegate](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManagerDelegate_Protocol/CLLocationManagerDelegate/CLLocationManagerDelegate.html)</td>
  </tr>
  <tr>
    <td>[RangeNotifier](http://developer.radiusnetworks.com/android-ibeacon-service/doc/com/radiusnetworks/ibeacon/RangeNotifier.html)</td>
    <td>[CLLocationManagerDelegate](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManagerDelegate_Protocol/CLLocationManagerDelegate/CLLocationManagerDelegate.html)</td>
  </tr>
</table>
  


## Known issues:

* [On the Nexus 4, Bluetooth and WiFi do not work properly at the same time.](https://code.google.com/p/android/issues/detail?id=41631)  If this library is used on the Nexus 4 when WiFi is active, it may cause a dropped WiFi connection, inability to do Wifi scans, delays in seeing iBeacons, or a total inability to see iBeacons.  Turning off WiFi on the Nexus 4 solves these problems.  Similar problems have been reported, but uncomfirmed, with Nexus 7 devices

* If two Applications are active simultenously doing Bluetooth scans, (including two that use this library), only partial scan data will get to each application, causing iBeacon detections to be dropped.

## License

This software is available under the Apache License 2.0, allowing you to use the library in your applications.
