---
layout: android-ibeacon-library
---

###Sending Notifications on iBeacon Detection

One of the most common use cases for iBeacons is to send a local notification to a phone when it is near an iBeacon.  This notification can be a sales or marketing message or an alert that a nearby service (like a taxi) is available.  Tapping on the notification launches your app and allows the user to see more information about the subject of the notification.

These notifications usually need to be sent when your app is not running.  This feature is automatic with iOS, but not Android, so setting it up is complex without the pro version of the Android iBeacon Library.

####How it works

The pro version of the Android iBeacon Library can launch your app into the background to start looking for iBeacons after the phone boots.  This will happen transparently with no visible user interface, while the rest of your app remains idle.

Once the desired iBeacon is detected, a callback method fires where you can push a custom notification message.  You can further configure the notification so it launches a specific part of your app when pressed.

####Will this feature drain batteries?

The library includes a battery saver that only looks for iBeacons every few minutes in order to minimize battery use.  The frequency of checks can be reduced as much as desired to save battery usage, or increased to improve responsiveness.

####How do I set this up?

The basic set up is the same for the [background launching example.](/ibeacon/android/samples.html)

The additional code needed to send a notification is shown in the [reference application.](https://github.com/RadiusNetworks/android-proximity-reference)
