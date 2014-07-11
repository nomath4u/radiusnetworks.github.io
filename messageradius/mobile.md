---
layout: messageradius
---

## Moble SDK

### Apple iOS

For Apple iOS devices, we recommend using bluetooth beacon detection.  This requires embedding the [Message Radius Client Library](http://app.messageradius.com/downloads/libMRClient.zip) in your app, and enabling it with the following calls:

    #import "MRClient.h"
    ...
    MRClient *_mrClient;
    ...
    // Put this in the AppDelegate's didFinishLaunchingWithOptions method
    _mrClient = [[MRClient alloc] init];

Once your app is running on a device, it will report any MessageRadius beacons it sees to the MessageRadius server, then fire any added and dropped subscriptions you set up to your servers.

In order to read the Apple Identifier for Advertisers (idfa) from the device, simply call:

    NSString* idfa = [[[ASIdentifierManager sharedManager] advertisingIdentifier] UUIDString];

Note: Wifi detection also works on iOS devices, but is not recommended.  As of iOS 7, it is no longer possible to read the wifi mac address of a device from an iOS app.  There is therefore no easy way to determine the wifi mac addresses to set up subscriptions in the Message Radius system.


### Android and other devices

For Android and other devices, we recommend using wifi detection.  This does not require embedding a dedicated client library inside an app.

In order to read the mac address of a device from your app on Android, simply call:

    String mac = context.getSystemService(Context.WIFI_SERVICE).getConnectionInfo().getMacAddress();

Note: While it is also possible to use beacon detection with Android devices with a low energy bluetooth chipset running Android 4.3 and later, only a small percentage of Android devices meet these criteria.

### Registring devices

Once you collect the device identifer (either the mac or the idfa), you can use this identifier to subscribe to location updates.

See the [Subscriptions Documentation](subscriptions.html) for details.
