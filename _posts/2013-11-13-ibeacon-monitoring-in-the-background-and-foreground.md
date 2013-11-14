---
layout: post
title: iBeacon Monitoring in the Background and Foreground
author: David G. Young
---

There has been lots of confusion about how iBeacon monitoring and ranging work in the background on iOS.  In general ranging only works in the foreground, but monitoring updates can happen in the background.  There are two ways to configure background monitoring, and even when done properly updates can take a long time to come.  Because these delays, some people mistakenly believe that monitoring in the background doesn't work.

##Summary

The tables below summarizes whether you can get monitoring updates under various conditions, and how long it might take to get them.

When the app is in the foreground:


Condition|Max time to detect a region change
--- | --- 
App ranging    |1 second 
App not ranging|up to 15 minutes 

When the app is not in the foreground:

Condition|Max time to detect a region change|
--- | --- |
Phone awakened, notifyEntryStateOnDisplay=YES|1 second 
Phone awakened, notifyEntryStateOnDisplay=NO|NEVER 
UIBackgroundModes=location ON|up to 15 minutes 
UIBackgroundModes=location OFF|up to 15 minutes 

Note: "Phone awakened" means pressing either the shoulder button or the home button when the phone display was off.  The detection times, in this case, reflect the time from when the screen turns on.


##How to maximize iBeacon responsiveness

The above tables show that two things are very important to maximizing the frequency of iBeacon monitoring updates:

1. For the fastest monitoring detection times in the foreground, do ranging whenever you are monitoring, even if you don't care about ranging updates.  Like this:

  ```
    CLBeaconRegion *region = [[CLBeaconRegion alloc] initWithProximityUUID:[[NSUUID alloc] initWithUUIDString:@"2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6"] major: 1 minor: 1 identifier: @"region1"];
    region.notifyEntryStateOnDisplay = YES;
    [_locationManager startMonitoringForRegion:region];
    [_locationManager startRangingBeaconsInRegion:region];
  ```

1. If you want to get a guaranteed extra monitoring update in the background whenever the users wakes up their phone, set the notifyEntryStateOnDisplay option on your region as so:

  ```
    CLBeaconRegion *region = [[CLBeaconRegion alloc] initWithProximityUUID:[[NSUUID alloc] initWithUUIDString:@"2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6"] major: 1 minor: 1 identifier: @"region1"];
    region.notifyEntryStateOnDisplay = YES;
    [_locationManager startMonitoringForRegion:region];
  ```

The tables show that the UIBackgroundModes=location in your application's {app}-Info.plist makes no difference.  Either way, you get monitoring updates in the background.


How do I know all this?  Because I tested lots of cases and measured how long it took to get a callback.  If you are skeptical, care to know the details, or want to peer review my work, read on.

## The Proof

To demonstrate how all this works, I built a simple iOS app to show what happens in the different cases in the table above.  It is a super simple app — basically just an [AppDelegate](https://github.com/RadiusNetworks/ibeacon-background-demo/blob/master/BackgroundDemo/BDAppDelegate.m) that starts monitoring and logs a line for every lifecycle or CoreLocation callback.  You can clone or browse the source code for the app here: [ibeacon-background-demo](https://github.com/RadiusNetworks/ibeacon-background-demo)

I ran this app on an iPhone 4S with iOS 7.0.2 while running a Radius Networks virtual iBeacon on my Mac.  The basic idea was that I used the log lines to measure whether monitoring callbacks happened (and how quickly) with the app in the foreground and in the background.  I also made slight changes to the code for each test case to show how changing various app configuration options affects the ability to monitor iBeacons.

For each of the test cases described below, the app was run as seen in the source code, with any alterations described.   I also explain any iBeacon conditions and then show an annotated copy of the log.  Any lines in the log that start with ** are an annotation and not something output by the app.

####TEST #1 - notifyEntryStateOnDisplay=YES
 
In this case, the app was run exactly as shown with each ranged region having the notifyEntryStateOnDisplay flag set to true.  The iBeacon was transmitting with the values for region1.

```
 2013-11-06 14:43:24.413 BackgroundDemo[385:60b] applicationDidFinishLaunching
 2013-11-06 14:43:24.435 BackgroundDemo[385:60b] applicationDidBecomeActive
 2013-11-06 14:43:25.474 BackgroundDemo[385:60b] locationManager didDetermineState INSIDE for region1
 2013-11-06 14:43:27.568 BackgroundDemo[385:60b] locationManager didDetermineState OUTSIDE for region2
 ** I hit shoulder button to turn off the screen
 2013-11-06 14:44:21.737 BackgroundDemo[385:60b] applicationWillResignActive
 2013-11-06 14:44:21.752 BackgroundDemo[385:60b] applicationDidEnterBackground
 ** I hit shoulder button to turn on the display and show the lock screen (but I do not unlock the phone)
 2013-11-06 14:44:27.776 BackgroundDemo[385:60b] locationManager didDetermineState INSIDE for region1
 2013-11-06 14:44:27.789 BackgroundDemo[385:60b] locationManager didDetermineState INSIDE for region1
 ** I hit shoulder button to turn on the display and show the lock screen (but I do not unlock the phone)
 2013-11-06 14:44:33.292 BackgroundDemo[385:60b] locationManager didDetermineState INSIDE for region1
 2013-11-06 14:44:33.322 BackgroundDemo[385:60b] locationManager didDetermineState INSIDE for region1
 ** I hit the home button to turn on the display and show the lock screen (But I do not unlock the phone)
 2013-11-06 14:46:40.204 BackgroundDemo[385:60b] locationManager didDetermineState INSIDE for region1
 2013-11-06 14:46:40.212 BackgroundDemo[385:60b] locationManager didDetermineState INSIDE for region1
```

Conclusion:  With the notifyEntryStateOnDisplay option set, each time you cause the screen to illuminate, the app gets a callback from LocationServices with the current state of of the monitoring regions.

####TEST #2 - notifyEntryStateOnDisplay=NO

In this case, the app was modified to set notifyEntryStateOnDisplay=false for all regions. The iBeacon was transmitting with the values for region1.

``` 
 2013-11-06 14:49:36.985 BackgroundDemo[400:60b] applicationDidFinishLaunching
 2013-11-06 14:49:37.007 BackgroundDemo[400:60b] applicationDidBecomeActive
 2013-11-06 14:49:38.045 BackgroundDemo[400:60b] locationManager didDetermineState INSIDE for region1
 2013-11-06 14:49:40.342 BackgroundDemo[400:60b] locationManager didDetermineState OUTSIDE for region2
 ** I hit the shoulder button to turn off the display
 2013-11-06 14:49:47.654 BackgroundDemo[400:60b] applicationWillResignActive
 2013-11-06 14:49:47.678 BackgroundDemo[400:60b] applicationDidEnterBackground
 ** I hit the shoulder button to turn on the display (but I do not unlock the phone)
 ** I hit the home button to turn on the display and show the lock screen (But I do not unlock the phone)
```

Conclusion:  With the notifyEntryStateOnDisplay option not set, waking up your phone will not give you a special ranging callback.

####TEST #3 - UIBackgroundModes=location 

In this case, the app was unmodified.  At the start of the test, the iBeacon was transmitting with the values for region1.  I then hit the shoulder button to put the phone to sleep.  In the middle of the test, I switched the iBeacon to transmit the values for region2.

```
 2013-11-06 14:52:47.718 BackgroundDemo[410:60b] applicationDidFinishLaunching
 2013-11-06 14:52:47.742 BackgroundDemo[410:60b] applicationDidBecomeActive
 2013-11-06 14:52:48.777 BackgroundDemo[410:60b] locationManager didDetermineState INSIDE for region1
 2013-11-06 14:52:51.101 BackgroundDemo[410:60b] locationManager didDetermineState OUTSIDE for region2
 ** I hit the shoulder button to turn off the display
 2013-11-06 14:53:21.989 BackgroundDemo[410:60b] applicationWillResignActive
 2013-11-06 14:53:22.004 BackgroundDemo[410:60b] applicationDidEnterBackground
 ** At 20:55:30, I turn off the iBeacon in region1 and turn one on in region2
 2013-11-06 15:07:48.843 BackgroundDemo[410:60b] locationManager didDetermineState INSIDE for region2
 2013-11-06 15:07:51.820 BackgroundDemo[410:60b] locationManager didDetermineState OUTSIDE for region1
```

Conclusion: The logs show that iOS made a callback with monitoring state with the app in the background.  The time delay between the actual region transition and when the callbacks got made was about 14 minutes.

####TEST #4 - UIBackgroundModes=location disabled

For this test, I repeat the previous test, but modify my app’s plist so that the “location” entry is not present in the UIBackgroundModes array.  At the start of the test, the iBeacon was transmitting with the values for region1. I then hit the shoulder button to put the phone to sleep.  In the middle of the test, I switched the iBeacon to transmit the values for region2.  

```
2013-11-06 20:45:37.255 BackgroundDemo[528:60b] applicationDidFinishLaunching
2013-11-06 20:45:37.279 BackgroundDemo[528:60b] applicationDidBecomeActive
2013-11-06 20:45:38.312 BackgroundDemo[528:60b] locationManager didDetermineState INSIDE for region1
2013-11-06 20:45:40.612 BackgroundDemo[528:60b] locationManager didDetermineState OUTSIDE for region2
** I hit the shoulder button to turn off the display
2013-11-06 20:45:49.322 BackgroundDemo[528:60b] applicationWillResignActive
2013-11-06 20:45:49.348 BackgroundDemo[528:60b] applicationDidEnterBackground
** At 20:26:00, I turn off the iBeacon in region1 and turn one on in region2
2013-11-06 21:00:38.379 BackgroundDemo[528:60b] locationManager didDetermineState INSIDE for region2
2013-11-06 21:00:41.354 BackgroundDemo[528:60b] locationManager didDetermineState OUTSIDE for region1
```

Conclusion:  Even with background mode location turned off, we still get a callback in the background.  

This surprised me.  Maybe the notifyEntryStateOnDisplay option enables background monitoring as well?  Let's check and see.

####TEST #4A - UIBackgroundModes=location disabled AND notifyEntryStateOnDisplay=NO
```
2013-11-06 21:08:29.415 BackgroundDemo[555:60b] applicationDidFinishLaunching
2013-11-06 21:08:29.438 BackgroundDemo[555:60b] applicationDidBecomeActive
2013-11-06 21:08:30.475 BackgroundDemo[555:60b] locationManager didDetermineState INSIDE for region1
2013-11-06 21:08:32.731 BackgroundDemo[555:60b] locationManager didDetermineState OUTSIDE for region2
2013-11-06 21:08:42.497 BackgroundDemo[555:60b] applicationWillResignActive
2013-11-06 21:08:42.514 BackgroundDemo[555:60b] applicationDidEnterBackground
2013-11-06 21:23:30.541 BackgroundDemo[555:60b] locationManager didDetermineState INSIDE for region2
2013-11-06 21:23:33.515 BackgroundDemo[555:60b] locationManager didDetermineState OUTSIDE for region1
```

Conclusion:   Even with background mode location turned off and notifyEntryStateOnDisplay turned off, we still get a callback in the background.  

####TEST #5 - Foreground Monitoring While Ranging

For this test, two lines of code are uncommented to enable ranging on the two regions that we are also monitoring.  The app is left in the foreground for the duration of the test.

```
 2013-11-06 18:35:45.763 BackgroundDemo[441:60b] applicationDidFinishLaunching
 2013-11-06 18:35:45.788 BackgroundDemo[441:60b] applicationDidBecomeActive
 2013-11-06 18:35:46.823 BackgroundDemo[441:60b] locationManager didDetermineState INSIDE for region1
 2013-11-06 18:35:49.136 BackgroundDemo[441:60b] locationManager didDetermineState OUTSIDE for region2
 ** At 18:36:45, I turn off the iBeacon in region1 and turn one on in region2
 2013-11-06 18:36:47.823 BackgroundDemo[441:60b] locationManager didDetermineState INSIDE for region2
 2013-11-06 18:36:51.822 BackgroundDemo[441:60b] locationManager didDetermineState OUTSIDE for region1
```

Conclusion:  As you can see, when ranging the monitoring callback comes super quickly -- about a second after I turned on the iBeacon for region 2.

####TEST #6 - Foreground Monitoring While NOT Ranging

For this test, ranging is not active, but the app is still left in the foreground for the duration of the test.

``` 
 2013-11-06 18:39:38.104 BackgroundDemo[459:60b] applicationDidFinishLaunching
 2013-11-06 18:39:38.126 BackgroundDemo[459:60b] applicationDidBecomeActive
 2013-11-06 18:39:39.166 BackgroundDemo[459:60b] locationManager didDetermineState INSIDE for region1
 2013-11-06 18:39:41.487 BackgroundDemo[459:60b] locationManager didDetermineState OUTSIDE for region2
 ** At 18:40:01, I turn off the iBeacon in region1 and turn one on in region2
 2013-11-06 18:54:39.212 BackgroundDemo[459:60b] locationManager didDetermineState INSIDE for region2
 2013-11-06 18:54:42.212 BackgroundDemo[459:60b] locationManager didDetermineState OUTSIDE for region1
```

Conclusion:  As you can see, when not ranging, the monitoring callback is delayed -- in this case it comes about 15 minutes after I turned on the iBeacon for region 2.

##Why does it work this way?

It is hard to be certain without seeing the iOS 7 source code, but I have an educated guess.

### Bluetooth Scan Rate

It is important to understand that the CoreLocation iBeacon APIs are mostly a thin layer on top of CoreBluetooth APIs. In order to see iBeacons, iOS starts a Bluetooth LE scan, runs it for a period of time, and looks for any devices that are discovered that contain the iBeacon signature.  (This is exactly what our Android iBeacon Library does to implement this functionality.)  Performing a scan, however, uses energy and the more frequently a scan takes place, the more drain on the phone battery.  iOS is probably altering the scan frequency in certain modes to save energy.  How long it takes to detect an iBeacon is really a function of how often iOS does a Bluetooth LE scan to look for them.

### Behavior When Ranging

When an app is doing ranging, it is clear that a continuous Bluetooth scan is going on.  An iBeacon sends out an advertisement once per second, and likewise iOS sends an app ranging data every second for each iBeacon.  It is also clear from the above tests that if an app is ranging, monitoring transitions happen more quickly.  If you are monitoring on a region and ranging on that same region simultaneously, then you turn on an iBeacon, within one second you will get both a ranging and a monitoring callback.  

### Behavior When not Ranging

If whatever app is in the foreground is not ranging for iBeacons, the monitoring callbacks take longer -- 15 minutes in the test above.  This suggests to me that if the foreground app is monitoring but not ranging, then iOS only does a Bluetooth scan every 15 minutes or so.  

However, this 15 minutes does not seem to be set in stone.   The exact time interval seems to vary in my experience.  In the tests above, this 15 minutes was longer than I had experienced in the past.   In earlier tests, I had never seen it take more than 4 minutes.  When I tried to reproduce some of the above tests a few days later, I got the same basic behavior, but I again got region change updates within 4 minutes.  Perhaps there are different states of the operating system or conditions (battery level, CPU usage, Display on, etc.) that affect this.  This definitely deserves more investigation.  Finding this out means performing lots more tests.

##Other important observations

1. If a monitored region in an app has region.notifyEntryStateOnDisplay = true, then each time the screen is turned on (by the shoulder button or the home button), then the app gets a callback in about a second.  In order for this to happen, iOS probably does a bluetooth scan right when the screen comes on and sends “in the region” notifications as soon as an iBeacon is seen, and “out of the region” notifications if no matching iBeacon is seen after a few seconds of scanning.  Notice that in all of the tests above "out of region" notifications always come a couple of seconds after "in the region" notifications.  This is probably because iOS doesn't consider an iBeacon to have disappeared until it doesn't show up in the scan results for a few cycles.

2. If you reboot the phone and start the app, no monitoring notifications are received for a few minutes after the phone boots, even though the bluetooth indicator is on.  My guess is that iOS doesn't start some parts of CoreLocation or CoreBluetooth right away.
