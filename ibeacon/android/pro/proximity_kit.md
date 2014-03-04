---
layout: android-ibeacon-library
---

###ProximityKit and the Pro Android iBeacon Library

ProximityKit is a cloud service with a client library for both Android and iOS.  The service lets you manage your app's iBeacons and associated data on central servers.  When you use the Pro Android iBeacon Library, you have full access to ProximityKit APIs and services.

####What is the difference between ProximityKit and the Pro Android iBeacon Library?

The Android version of the ProximityKit client library is the same binary file as the Pro version of the Android iBeacon Library.  Both are covered by the same license.  For Android, the two are the same software. 

####Do I have to use the ProximityKit cloud service?

No.  The cloud service allows features such as associating data with iBeacons and setting up iBeacon regions in the cloud.  But if you do not use these features, the Pro Android iBeacon Library still provides other premium features like launching in the background, sending notifications, and the battery saver.

####Are the APIs different?

ProximityKit APIs are built on top of the Android iBeacon Library APIs and offer a more streamlined setup for simple use cases based on the ProximityKitManager class.   Those needing greater control should use the Pro Android iBeacon Library's IBeaconManager class.

####Can I use both together?

If you are using the ProximityKitManager, you should generally avoid accessing the IBeaconManager.  The ProximityKitManager is a layer above the IBeaconManager, so it is easy to break the configuration if you first set up the ProximityKitManager then later make modifications with the IBeaconManager.

####Can I access cloud data without using the ProximityKit API?

Yes.  This is possible with the IBeaconManager.  See the accessing iBeacon data example code [here.](/ibeacon/android/samples.html) 

####Which API should I use?

Use the ProximityKitManager if you have a simple iBeacon use case that displays an activity or notification based on seeing iBeacons and associated data pre-configured in the cloud.  For more complex use cases, especially if you need runtime control  over the iBeacon ranging and monitoring configurations, use the IBeaconManager.

