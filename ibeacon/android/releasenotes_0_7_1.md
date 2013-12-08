---
layout: android-ibeacon-library
---


###Release 0.7.1

* Provides control over the frequency of the Bluetooth scan cycle in both the foreground and the background.
* Supports configuration of Simulated Scan Data, useful for testing in an emulator on or devices without Bluetooth LE.
* Fixes an applicaton not responding condition when Bluetooth is turned off after the library has started scanning for Bluetooth devices.
* Supports manifest merging so the AndroidManifest.xml no longer needs to be modified to set up the iBeacon Service.
