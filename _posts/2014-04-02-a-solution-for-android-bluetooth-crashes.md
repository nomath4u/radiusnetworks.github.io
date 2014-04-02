---
layout: post
title: A Solution for Android Bluetooth Crashes
author: David G. Young
---

Lots of folks using iBeacons with Android devices have been suffering from repeat error dialogs saying "Unfortunately, Bluetooth Share has Stopped".  These annoying dialogs are caused by low-level [bug #67272](https://code.google.com/p/android/issues/detail?id=67272) in the Bluedroid stack, which is a layer between the operating system and the Bluetooth hardware.  

While the bug has been around since Bluedroid was put into Android 4.2 over a year ago, reports only started commonly showing up recently.  Today, some folks report that the dialog appears so frequently when doing Bluetooth LE scans, that the technology is effectively unusable.   What changed?

Some companies are now distributing beacons that are a significant cause.  By default, the beacon shown below has a proprietary security feature that rotates its Bluetooth MAC address every 0.8 seconds.  This creates big problems for Android devices doing Bluetooth scans.  Due to the bug mentioned above, Bluedroid can only handle seeing 1,990 different Bluetooth MAC addresses before the Android BluetoothService crashes.  This might take months or years under normal conditions, but if the little blue devil below is around, spitting out a new MAC address every 0.8 seconds, this only takes 42 minutes!

<img src='/img/bluetooth-crash.jpg'/>

What's worse is that Android periodically saves its list of recently seen Bluetooth devices to internal storage, and reloads it after a crash.  Eventually, this stored list will contain all 1,990 allowed MAC addresses, causing the Android BluetoothService to crash as soon as it sees a single new device.  Once this state is reached, Android devices are completely unable to scan for new Bluetooth LE devices without an immediate crash of the BluetoothService.  Lots of frustrated users have reported having to do a factory reset to clear the condition.

The good news is that we have a solution.  It’s not a fix precisely, because that requires an update to the Bluedroid stack, which needs a new release of Android. But we have built a [Bluetooth Crash Resolver](https://github.com/RadiusNetworks/bluetooth-crash-resolver) software module that can both clear the condition, and usually prevent it from happening in the first place.

The solution involves tracking the number of Bluetooth devices detected, and when it gets large enough, forcing a flush of Bluedroid’s internal list.  Commanding a direct flush requires system-level access, something that Android’s security restrictions don’t allow.  But we can command a Bludroid "discovery" process, something that just so happens to pare down the list of recently seen devices to the most recent 256.  There are some other technical details, but the end result is that this can prevent the crashes from ever happening in the first place.  And if a crash does happen, our software module can detect it and clear out the list so it doesn’t happen again.

This module has already been integrated into the [Android iBeacon Library version 0.7.6](http://developer.radiusnetworks.com/ibeacon/android/download.html) as well as our widely-used [iBeacon Locate](https://play.google.com/store/apps/details?id=com.radiusnetworks.ibeaconlocate) Android app.  We’re also releasing it as open source so authors of other Bluetooth LE apps and other iBeacon frameworks, can use it as well.  

The module isn’t a perfect solution.  It relies on Bluedroid’s discovery process, which consumers a lot of power and can terminate established Bluetooth connections.  But these side effects are preferable to the service crashing entirely (which terminates Bluetooth connections anyway.)  And the additional battery drain should be minor unless you hang around doing a constant scan in the presence of the photographed beacon all day, behavior which would cause your battery to go dead even without this module.   

The module is still a bit experimental, so we will be refining it based on feedback and suggestions from folks like you.  But even with these caveats, it finally makes it possible to live with bug #67272 until it is fixed in a future release of Android.

For those of you who have a device in this condition now, you have two ways to clear the condition.  First, if you are using Locate for iBeacon, upgrading and using the latest copy should solve the problem so you don’t get crashes anymore when using that app.

If you are using other apps that do Bluetooth LE scans, you can download our standalone [BLE Crash Resolver](https://play.google.com/store/apps/details?id=com.radiusnetworks.bluetoothcrashresolver) app.  It won’t completely prevent crashes -- that is only possible by embedding our module inside an app itself.  But the standalone app will watch for crashes and clear the condition as soon as one happens, so that the next crash will be as far in the future as possible. 

