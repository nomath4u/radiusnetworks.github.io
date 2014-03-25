---
layout: android-ibeacon-library
---

##Android Bug 67272
###"Unfortunately, Bluetooth Share has Stopped"

####Summary

Some users using the Android iBeacon Library directly, or inside apps that use it like iBeacon Locate and the Beacon Scavenger Hunt, report seeing repeat dialogs saying "Unfortunately, Bluetooth Share has stopped."
This dialog will continue to appear periodically when the app is running.  It will also appear when the Android Bluetooth Settings page is up, or when running an app using the
Estimote Android SDK, or any other app that does Bluetooth scanning.  During times that the dialog appears, Bluetooth scan results and iBeacon detections will
not get correct results.  

Once a phone gets into this state, it will remain in this state until Bluetooth is turned off and back on.  Rebooting the phone will not clear the state.

####Cause

The problem is caused by a low-level Android bug that does not properly handle an internal buffer holding recently scanned Bluetooth LE Mac addresses.  Once the buffer fills up, Android's 
BluetoothService crashes and restarts itself.  The same problem happens the next time a Bluetooth device is scanned, causing repeated crashes and restarts of the BluetoothService.
The "Unfortunately, Bluetooth Share has Stopped" dialog is a side effect, caused by a second bug Android's Bluetooth Share service, which does not cleanly handle the restart of the Bluetooth Service, causing itself to crash as well.

####Impact

The good news is that this bug largely affects developers or people who work in Bluetooth dev shops who have been around a large number of Bluetooth devices in the same place. Going to a BLE or iBeacon hackathon will almost certainly a trigger it. Fortunately, most end users do not do this.  

The bug can also be triggered by performing Bluetooth scans near beacons that send out randomized mac addresses with each transmission, such as Gimbal beacons.

####Solutions

The following procedure is known to resolve the problem at least temporarily.  The problem may return in hours or days, depending on how much time the device spends doing Bluetooth scans
in the vicinity of devices that randomize their Bluetooth Mac address.

Step 1. Reboot your phone (optional, but it makes the next step easier)

Step 2. Go to Settings -> Apps -> Running and stop any apps that may perform Bluetooth scans or use iBeacons.

Step 3. Turn off Bluetooth.

Step 4. Turn on Bluetooth.

It is very important to exit all applications that perform Bluetooth scans, otherwise the above procedure will not work.  Performing a factory reset is unnecessary.

Radius Networks is currently researching automated workarounds.

A fix for the low-level bug will require either an Android update or an update to the pre-installed Bluetooth apps.


####More Information

Google bug report: 

[https://code.google.com/p/android/issues/detail?id=67272](https://code.google.com/p/android/issues/detail?id=67272)

Detailed description of the Android bug:

[http://stackoverflow.com/questions/22048721/bluetooth-share-has-stopped-working-when-performing-lescan](http://stackoverflow.com/questions/22048721/bluetooth-share-has-stopped-working-when-performing-lescan) 

Android iBeacon Library GitHub issue:

[https://github.com/RadiusNetworks/android-ibeacon-service/issues/16](https://github.com/RadiusNetworks/android-ibeacon-service/issues/16)
