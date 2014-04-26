---
layout: android-ibeacon-library
---

## Beacon Simulator Sample Code


You may simulate detection of iBeacons by creating a class within your project that overwrites the BeaconSimulator 
class found in the Android iBeacon Library. This is especially useful for when you are testing in an Emulator or on a 
device without BluetoothLE capability. There is a sample of this built into the [Reference application for the Android iBeacon Service](http://github.com/RadiusNetworks/android-ibeacon-reference/) 
called [TimedBeaconSimulator](https://github.com/RadiusNetworks/android-ibeacon-reference/blob/master/src/com/radiusnetworks/ibeacon/TimedBeaconSimulator.java).

To initialize a BeaconSimulator, place the following code within the onCreate method of your Activity, 
making sure to place your Beacon Simulator class name within the < >.
```
  IBeaconManager.setBeaconSimulator( new <YourBeaconSimulatorClass>() );
```

This will create an instance of your BeaconSimulator class and set it within the iBeacon library. From that point on, 
any iBeacons returned by the getBeacons method in your BeaconSimulator class will become visible to your application.
For this reason, the getBeacons method is a required method for the BeaconSimulator class. Any changes to the list 
of iBeacons returned by the getBeacons method will be reflected immediately in your local environment. 
This allows you to make dynamic changes to what iBeacons appear during your testing at any given time. In the event
that there are live iBeacons within your radius and you are running a device that has the ability to sense them, the
simulated iBeacons will appear right alongside the real ones.

Please note that any Simulated Beacons will be ignored by the Android iBeacon Library when the app is in production mode.
