---
layout: ibeacon
---

## Software Filter Example Code

The following example code shows a variable and methods you would add to your AppDelegate class to keep track of whether you have seen
specific iBeacons before, and how long ago it was.  In this example, a method is called to send a local notification only if a particular
iBeacon had not been seen in the past 24 hours.


    NSMutableDictionary *beaconLastSeen;

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
        beaconLastSeen = [[NSMutableDictionary alloc] init];
    }

    - (void)locationManager:(CLLocationManager *)manager didRangeBeacons:(NSArray *)beacons inRegion:(CLRegion *)region
    {
        for (CLBeacon *beacon in beacons) {
            Boolean shouldSendNotification = NO;
            NSDate *now = [NSDate date];
            NSString *beaconKey = [NSString stringWithFormat:@"%@_%ld_%ld", [beacon.proximityUUID UUIDString], (long) beacon.major, (long) beacon.minor];
            NSLog(@"Ranged UUID: %@ Major:%ld Minor:%ld RSSI:%ld", [beacon.proximityUUID UUIDString], (long)beacon.major, (long)beacon.minor, (long)beacon.rssi);
        
            if ([beaconLastSeen objectForKey:beaconKey] == Nil) {
                NSLog(@"This beacon has never been seen before");
                shouldSendNotification = YES;
            }
            else {
                NSDate *lastSeen = [beaconLastSeen objectForKey:beaconKey];
                NSTimeInterval secondsSinceLastSeen = [now timeIntervalSinceDate:lastSeen];
                NSLog(@"This beacon was last seen at %@, which was %.0f seconds ago", lastSeen, secondsSinceLastSeen);
                if (secondsSinceLastSeen < 3600*24 /* one day in seconds */) {
                    shouldSendNotification = YES;
                }
            }
        
            if (shouldSendNotification) {
                [self sendLocalNotification];
            }
        }
    }