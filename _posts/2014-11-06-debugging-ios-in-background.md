---
layout: post
title: Debugging iOS Apps in the Background
author: Christopher Sexton and Scott Yoder
---

Most mobile developers know that iOS will wake up their app in some cases and notify it of some event, things like geofence or beacon regions can cause this to happen. What's interesting is that iOS not only notify an app that is sleeping in the background, but also apps that have been forcefully killed by the user, or never restarted after a reboot of the phone.

This is all good until you need to debug your app in this state. Turns out it is pretty straightforward, but required a little trick or two to get Xcode to attach the debugger to the process.

Here's how to debug this in Xcode. First you need an app that is setup to get a callback from iOS that can happen while it is in the background. In this case I am using ProximityKit and an iBeacon to trigger that behavior.

## Step 1: Get the process name.

This is printed out in the log any time you call `NSLog()`.

I just dropped that line in the did finish launching delegate:

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  NSLog(@"Starting Up...");
  // ...
}
```

Then you can run that app and grab the Process Name.

![Grab the app name from the log](/img/background-debug-1-grab-app-name-from-log.png)

## Step 2: Stop your app

Stop it in Xcode. I also tend to manually re-launch it on the phone and then force kill it by swiping it off in the app switcher, just to be sure.

![stop the app](/img/background-debug-2-stop-the-app.png)

## Step 3: Set the breakpoint

Create the breakpoint in the delegate that iOS will call when it wakes your app.

![set breakpoint](/img/background-debug-3-set-breakpoint-in-background-method.png)


## Step 4: Attach to the Process

Now for the tricky part. You need to attach to the process, but the process isn't running yet. So, we will use the process name. On the menu bar choose `Debug > Attach to Process > By Process Identifier (PID) or Name...` and enter the process name you found in Step 1.

![attach to process](/img/background-debug-4-attach-to-process.png)

## Step 5: Fire the event

For this app I just turned on the iBeacon which caused iOS to wake my app and Xcode attached to the process and enabled the debugger at the break point.

Once I enabled the beacon, waited a couple seconds and iOS dutifully started the app and called the delegate.

![breakpoint hit](/img/background-debug-5-hit-breakpoint-after-launching.png)

This little trick has been extremely handy in tracking down some changes in the way iOS 8 handles permissions.

Pro-tip: You need to use [NSFileProtectionNone](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSFileManager_Class/index.html#//apple_ref/swift/data/NSFileProtectionNone) if you have to load a file cache while in the background.



