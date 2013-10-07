---
layout: proximitykit
---

# Getting started with Proximity Kit

This guide should get your app working with Geofencing and the Proximity Kit service.

## Download and Install

If you have not downloaded the Proximity Kit framework and added it to your project you should [go do that first](http://proximitykit.com/download).

## Creating a Location Manager

Proximity Kit adds a thin wrapper around the standard Location Manager. This allows for automatic registering of fences, but should give your app all the power and control it needs to use location data.

First, we need to create a instance of the `PKLocationManager`. Each app should have only one instance of this object. For simple applications we recommend this to be an instance variable on your Application Delegate, however works just fine as long as the object is around for the lifetime of the application. For simplicity sake this document will describe setting up the `AppDelegate` class to work with the `PKLocationManager` instance.

In `AppDelegate.h` add the interface for `PKLocationManagerDelegate` and a property to store an instance of the manager:

```objective-c
#import <UIKit/UIKit.h>
#import <ProximityKit/ProximityKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate, PKLocationManagerDelegate>

@property (strong, nonatomic) UIWindow *window;
@property PKLocationManager *locationManager;

@end
```

In `AppDelegate.m` allocate the manager and assign the delegate:

```objective-c
- (BOOL)application:(UIApplication *)application
        didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    self.locationManager = [[PKLocationManager alloc] init];
    [self.locationManager setDelegate:self];

    return YES;
}
```

## Create the delegate methods

Within your AppDelegate class you can define either the enter or exit region methods, which will be called when the app enters or exits a fence.

```objective-c
- (void)locationManager:(PKLocationManager *)manager didEnterRegion:(NSString *)region {
    NSLog(@"entered %@", region);
}
- (void)locationManager:(PKLocationManager *)manager didExitRegion:(NSString *)region {
    NSLog(@"exited %@", region);
}
```

## Simulating the Fences

In the iPhone simulator you can change the location in the menu "Debug -> Location -> Custom Location...".

From there you will have a form where you can enter GPS coordinates of you location.

Note: If the location is set to coordinates within your fence when the simulator is launched you may have to set it out side of the fance and back inside again.



