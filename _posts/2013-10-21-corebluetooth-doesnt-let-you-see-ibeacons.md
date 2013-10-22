---
author: David G. Young
layout: post
title: CoreBluetooth Doesn't Let You See iBeacons
---

One of the features of Radius Networks' [Android iBeacon Library](http://developer.radiusnetworks.com/ibeacon/android/) is the ability to monitor or range for any iBeacon regardless of its ProximityUUID.  
Essentially, you can construct a Region object (the Android Library's equivalent of iOS' CLBeaconRegion) with the ProximityUUID set to null, allowing you to
see any iBeacon.

This begs the question, is there any way to do this on iOS?  The short answer is no.  The long answer involves trying a couple of different ways:

## CoreLocation APIs

Using the CoreLocation APIs, the obvious way to look for all iBeacons is to pass a nil value for the ProximityUUID when constructing a CLBeaconRegion.  But that doesn't work.  If you try this:

```
CLBeaconRegion *region = [[CLBeaconRegion alloc] initWithProximityUUID:nil identifier:@"myUniqueIdentifer"];
```

You will get:

```
*** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '*** -[__NSArrayM insertObject:atIndex:]: object cannot be nil'
```

Clearly, that doesn't work.  It's time to try something else.

## CoreBluetooth APIs

Another alternative is to try the CoreBluetooth APIs to see iBeacons.  Since iBeacons are simply BluetoothLE advertisements, it seems reasonable that we could just look for advertisements with these APIs, and read out the identifiers.
This is exactly what the Android iBeacon Library does.

I wrote up a simple ViewController that tries to do this.  Here is an excerpt:

```

...

- (void)viewDidLoad
{
    [super viewDidLoad];
    NSLog(@"loaded test view for finding beacons using core bluetooth");
    _manager = [[CBCentralManager alloc] initWithDelegate:self queue:nil];
}


- (void)centralManagerDidUpdateState:(CBCentralManager *)central
{
        switch (central.state) {
            case CBCentralManagerStatePoweredOn:
            {
                NSDictionary *options = [NSDictionary dictionaryWithObjectsAndKeys:[NSNumber numberWithBool:YES], CBCentralManagerScanOptionAllowDuplicatesKey, nil];
                
                [_manager scanForPeripheralsWithServices:nil options:options];
                 NSLog(@"I just started scanning for peripherals");
                
                break;
            }   
                
        }
}
    
- (void)centralManager:(CBCentralManager *)central didDiscoverPeripheral:(CBPeripheral *)peripheral advertisementData:(NSDictionary *)advertisementData RSSI:(NSNumber *)RSSI {
        
    NSLog([NSString stringWithFormat:@"I see an advertisement with identifer: %@, state: %@, name: %@, services: %@,  description: %@",[peripheral identifier],[peripheral state],[peripheral name],[peripheral services],[advertisementData description]]);
    NSString *key;

    if (_peripheral == nil) {
        NSLog(@"Trying to connect to peripheral");
        _peripheral = peripheral;
        _peripheral.delegate = (id) self;
        [central connectPeripheral:_peripheral options:nil];
    }
}

-(void)centralManager:(CBCentralManager *)central didConnectPeripheral:(CBPeripheral *)peripheral{
    if (peripheral == nil) {
        NSLog(@"connect callback has nil peripheral");
    }
    else {
        NSLog([NSString stringWithFormat:@"Connected to peripheral with identifer: %@, state: %d, name: %@, services: %@",[peripheral identifier],[peripheral state],[peripheral name],[peripheral services]]);
        
        NSLog(@"discovering services...");
        _peripheral = peripheral;
        _peripheral.delegate = (id) self;
        [_peripheral discoverServices:nil];
    }
}

-(void)peripheral:(CBPeripheral *)peripheral didDiscoverServices:(NSArray *)serviceUuids {
    NSLog(@"discovered a peripheral's services: %@", serviceUuids);
}


```

This code does the follwing:

1. It initializes the CBCentralManager in the viewDidLoad method.
2. Once CBCentralManager is intitialized, the centralManagerDidUpdateState method starts scanning for any BluetoothLE peripheral, regardless of offered services.
3. For each BluetoothLE advertisement that is seen, the didDiscoverPeripheral method is called, which logs everything visible about that peripheral.
4. That same method then requests discovery of services for that peripheral, which are logged in the didDiscoverServices method.

I ran this code on an iPhone while an iBeacon was transmitting in the vicinity, then looked at the logs.  As expected, I did see something show up in the logs once per second,
and only when that iBeacon was transmitting.  What I saw, however, wasn't very useful:

```
2013-10-21 13:24:27.181 iBeacon Tester[558:60b] loaded test view for finding beacons using core bluetooth
2013-10-21 13:24:27.206 iBeacon Tester[558:60b] I just started scanning for peripherals
2013-10-21 13:24:33.041 iBeacon Tester[558:60b] I see an advertisement with identifer: <__NSConcreteUUID 0x1653a960> 1DA1386A-9830-FC00-59F8-CBC1A22531BB, state: (null), name: (null), services: (null),  description: {
    kCBAdvDataChannel = 38;
    kCBAdvDataIsConnectable = 1;
}
2013-10-21 13:24:33.043 iBeacon Tester[558:60b] Trying to connect to peripheral
2013-10-21 13:24:35.620 iBeacon Tester[558:60b] Connected to peripheral with identifer: <__NSConcreteUUID 0x1653a960> 1DA1386A-9830-FC00-59F8-CBC1A22531BB, state: 2, name: (null), services: (null)
2013-10-21 13:24:35.621 iBeacon Tester[558:60b] discovering services...

```

The advertisement mentioned in the logs is indeed the iBeacon's advertisement.  If I stopped the iBeacon, I didn't see it.  But the UUID logged has nothing to do with the iBeacon's
proximityUUID.  And after writing out to the log every other field accessible from the CBPeripheral class, it was clear that nothing would give me the ProximityUUID, the major or the minor
fields of the iBeacon.  

Just for fun, I also added code to request a list of services for the CBPeripheral.  (A more functional BluetoothLE device can be queried for the services it offers, but iBeacons are transmit only and don't respond to these queries.)  
Not surprisingly, the iBeacon CBPeripheral doesn't give you a list of sevices.  The didDiscoverServices method never got called.

## Conclusions

1. There doesn't seem to be any hack to look for an arbitrary iBeacon on iOS -- you have to know at least the ProximityUUID to see one.
2. Using CoreBluetooth is pretty useless for working with iBeacons.  You can see their advertisements, and measure the signal strength, but you can't see any of the identifiers, and thus, you don't even know for sure if any advertisement you see is an iBeacon at all, versus any other BluetoothLE device.




