---
layout: post
title: Extending Background Ranging on iOS
author: David G. Young and Scott Yoder
---

One of the most common use cases for beacon-enabled applications is to perform an action when a user gets close to a specific location.  Since beacons typically have a transmitting range of up to 50 meters, it’s often not appropriate to trigger that far away.  A gas station app, for example, shouldn’t prompt a user to pay at the pump when the user’s phone detects the beacon from across the street.

There are a couple of strategies to solve this.  One involves turning down the transmitter power of the beacon so it can’t be detected until the user is only a few meters away.  The RadBeacon models sold by Radius Networks allow adjusting the transmitter power for this exact reason.   But when very close triggering is desired, this approach can sometimes lead to actions not being triggered at all.  Received signal levels can vary depending on mobile device model, and where the device is placed.  

A second approach involves tracking the beacon in the background, noting its estimated distance, and only triggering an action when the beacon is estimated to be within a specific range.  This approach is problematic on iOS, because CoreLocation generally allows only 10 seconds of ranging time when an app is in the background.  If a beacon is first detected at 50 meters, and a person is approaching the beacon at one meter per second, the mobile device will still be 40 meters away when iOS suspends the app and stops it from ranging.

The good news is that it is possible to extend background ranging time on iOS.  If your app is a navigation app, you can specify location updates in the “Required background modes” in your Info.plist.  But this approach makes it harder to get AppStore approval -- you have to convince reviewers that your app is providing navigation services to the user.  This probably isn’t true for many apps that simply want to use beacons to trigger at a specific distance.

Fortunately, you can still extend background ranging time without requesting special background modes.  The time you can get is limited -- only three minutes.  But this clock restarts each time your app is woken up in the background, meaning you can get an extra three minutes of ranging time each time your app detects a beacon (enters a beacon region) or stops seeing beacons (exits a beacon region.)

##How to Get Extra Ranging Time

Any app can request extra background processing time by calling `UIApplication beginBackgroundTaskWithName: expirationHandler:`.  Once this call is made, the app can check to see how much running time is available by checking the `backgroundTimeRemaining` property on `UIApplication`.  If your app is in the foreground, or if already has normal permission from the operating system to run (e.g. for the first 10 seconds after detecting a beacon), this method will return a very large number (max double), effectively telling you that you can run forever. 

If normal permission to run in the background has ended, this method will return a value in seconds that starts counting down from 180.  Until it reaches zero, your app can continue ranging for beacons, at which time the app will be suspended by the operating system.

You can see an example of how to set this up in the code block below.  The method `extendBackgroundRunningTime` will request extra background time and then set up a dummy background task that doesn’t do anything besides sleep and log information to note what it is doing.  Once the time is about to expire, the code gets an expirationHandler callback from the operating system, and kills the dummy background task.  In the example below, the extra time is requested whenever the app enters the background, or whenever a beacon region transition occurs (e.g. whenever the app starts seeing or stops seeing beacons.)


```
- (void)locationManager:(CLLocationManager *)manager didDetermineState:(CLRegionState)state forRegion:(CLRegion *)region
{
    if (_inBackground) {
        [self extendBackgroundRunningTime];
    }
}

- (void)applicationDidEnterBackground:(UIApplication *)application
{
    [self extendBackgroundRunningTime];
    _inBackground = YES;
}

- (void)applicationDidBecomeActive:(UIApplication *)application
{
    _inBackground = NO;
}

- (void)extendBackgroundRunningTime {
    if (_backgroundTask != UIBackgroundTaskInvalid) {
        // if we are in here, that means the background task is already running.
        // don't restart it.
        return;
    }
    NSLog(@"Attempting to extend background running time");
    
    __block Boolean self_terminate = YES;
    
    _backgroundTask = [[UIApplication sharedApplication] beginBackgroundTaskWithName:@"DummyTask" expirationHandler:^{
        NSLog(@"Background task expired by iOS");
        if (self_terminate) {
            [[UIApplication sharedApplication] endBackgroundTask:_backgroundTask];
            _backgroundTask = UIBackgroundTaskInvalid;
        }
    }];
    
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        NSLog(@"Background task started");
        
        while (true) {
            NSLog(@"background time remaining: %8.2f", [UIApplication sharedApplication].backgroundTimeRemaining);
            [NSThread sleepForTimeInterval:1];
        }
        
    });
}
```

You can see a full reference application that demonstrates this functionality [here.](https://github.com/RadiusNetworks/ibeacon-background-demo/blob/background-task) The README file for this application shows log output from test runs, demonstrating that the app is able to range in the background for thee minutes after each beacon detection.

When setting this up, it is very important that you stop the background task when the expirationHandler is called by iOS, otherwise your app will crash when the background running time reaches zero.  If this happens, your app won’t be detecting any more beacons until the user manually launches the app the next time.

##Other Uses for beginBackgroundTask

While this technique works well to extend the background ranging period for beacons, it’s worth noting that there are plenty of other uses as well. Any long running task that would fail if interrupted can benefit from this.  One example is web-service calls.  If the app is sent to the background before the response is received the app may need to re-issue the request once it starts again.  Not only will this cause a delay to your functionality that happens after the web service completes, but your web server will now have to handle an extra request once your app starts again. 

##Is This Allowed by Apple?

It is important to note that this technique does not require any special background modes or any special AppStore approval.  There doesn’t appear to be any policy limit on requesting this extra three minutes of background running time, you can do it whenever you want.  You just need to understand that iOS enforces limits by only granting a maximum of 180 seconds of extra running time before the app would otherwise have been suspended.  For those of us building beacons applications, this means we can range for three minutes after detecting a beacon.

Is this three minutes guaranteed, and is it possible to get more?  On iOS 8 and iOS 7, the operating system consistently grants an extra three minutes of background running time.  But that doesn’t mean Apple can’t change this policy with a future update.  On iOS 6, apps could get 10 minutes of background running time using this technique, but Apple quietly reduced this to three minutes starting with iOS 7.  It is quite possible the maximum running time could be reduced in a future operating system update.

And while there doesn’t appear to be any official way to get more than three extra minutes of background running time (without declaring background modes), [some folks have reported tricks to game the system and extend this further.](http://gooddevbaddev.wordpress.com/2013/10/22/ios-7-running-location-based-apps-in-the-background/)  Just be aware that if you take such an approach, you risk having your app rejected during review -- this is probably not a good idea.

##Conclusion

By starting a background task, beacon apps can range in the background for up to three minutes each time the app starts or stops seeing a specific beacon region.  While this isn’t a perfect solution to the problem of needing to range in the background, it does open up many possibilities.  In the gas station use case we mentioned above, if a mobile device that detects the beacon 50 meters away, and approaches the pump at one meter per second, the app will still be ranging in the background when the user is right at the pump.  The user can then get the expected experience of being prompted to pay at the pump, instead of awkwardly being prompted to pay when approaching the station from across the street.

Radius Networks is committed to finding solutions for such real-world use cases.  By combining well-known technologies with deep experience to make them work, things that initially seem impossible can actually become reality.


