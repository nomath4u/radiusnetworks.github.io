---
layout: ibeacon
---

#Beacon Development Kit Instructions (Version 3.0)

Note: These instructions are for a previous version of the Beacon Development Kit (shipped May. 2014 - Jun. 2014). For instructions on the latest version (shipping July, 2014) [click here](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit-instructions.html). 



##Getting Started

###Picking a power supply

The Beacon Development Kit requires a standard 5V power supply with a micro USB connector. The Raspberry Pi computer
can be sensitive to poorly performing chargers. If your charger works, you should see a red LED illuminated when the
board is powered, and a green LED that flashes intermittently. If this doesn't happen, try a different charger.

###Verifying connections

Verify that the SD card and the Bluetooth dongle are properly seated in their sockets, and the power supply is connected
to a wall socket and the micro USB connector on the board.

###Powering on the Beacon

There is no power switch for the Beacon Development Kit. As soon as the 5V micro USB adapter is connected to a wall
socket and the computer board, the beacon will start up automatically. It takes about 60 seconds for it to begin
transmitting.

###Verifying it Works

Use a beacon test program like Radius Networks’ “Locate iBeacon” app for iOS to verify the
iBeacon is transmitting. [Click here](http://store.radiusnetworks.com/collections/all) for information on how to download these free tools. 

##Information on Beacon Identifiers

Beacons have a three part identifier consisting of ProximityUUID, Major, and Minor. By default, the Beacon Development
Kit will transmit with a ProximityUUID of 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6, Major of 1, and Minor of 1.  If you have
the Dual Beacon Development Kit model, it will also transmit as a second iBeacon with 
ProximityUUID of 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6, Major of 1, and Minor of 2.  These values are included in the 
default identifier list in Radius Networks’ beacon test apps for iOS and Android so these tools can be used to test 
if the eacon is working.

The ProximityUUID is a 16 byte identifier, usually expressed as a series of hexadecimal digits separated by dashes. 
Generally, you generate one ProximityUUID to use in all of your organization's beacons. If you don't have one already,
you can use any GUID/UUID generation tool (for example: the uuidgen command in OS X) to create your own identifier
similar to the one listed above.  Radius Networks’ beacon test apps (listed above) also have tools to generate your own
UUID.

The Major and Minor are both integers that can be between between 0 and 65535. The Major is used to put beacons into a
logical group, like a building or a room. The Minor is used to identify an individual beacon within the group.

Power is a measure of signal strength received by a device one meter from the beacon.  This is used by another device
to calibrate distance estimates from the beacon.  The Beacon Development Kit comes preconfigured with the default 
value for the included bluetooth adapter (-59) so you only need to change this value if using another adapter.  If you
decide to use another adapter, you can use Radius Networks’ beacon test apps to find its calibrated power value.

Broadcast Frequency indicates how often the beacon will broadcast its advertisement -- in times per second 
(Max = 10, Min = 0.25).  With this new feature you can select how often the beacon broadcasts, which will help you 
determine how your app will respond to beacons with different advertising rates.  By default this value is set to 10 
times per second, which is the setting recommended by Apple engineers.

##Customizing the Beacon identifiers

With the Radius Networks Beacon Development Kit, you can easily customize the identifiers being broadcast by the 
beacon.  The easiest way to do this is with a card reader and a computer (if your computer doesn’t have an SD card 
reader skip to the next section to connect to the Raspberry Pi and edit these values in the console). Unplug the 
beacon, then remove the SD card and put it into your computer's SD card reader. After attaching it to your computer, 
look for a volume with a file named ibeacon.conf in the root directory. When you open this file, you should see two 
blocks like the one below, these give the identifiers for each transmitter (Note: changing the second set of identifiers
does nothing unless you have the dual model).

```    
export UUID1=2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6
 export MAJOR1=1
 export MINOR1=1
 export POWER1=-59
 export BROADCAST_FREQUENCY1=10
```

To change beacon identifiers simply edit the values for each of these parameters.  Just paste your UUID in the standard
format (both uppercase and lowercase letters are acceptable) and enter the other values in decimal format.  Be sure to 
properly save the file and detach the SD card from your system before ejecting the card, otherwise the card may be 
corrupted.  Note: if you change to a custom UUID you will need to add this UUID to your ‘Locate iBeacon’ iOS app 
for it to be visible since iOS devices can only detect beacons with known UUIDs.

###Next Steps

Now it's time to start developing your app! When you are ready to deploy your iBeacons, visit [our products page](http://store.radiusnetworks.com/collections/all) to buy models suitable for production use.

##Controlling the Beacon(s) Manually

While developing, you may find it useful to start and stop the beacon without having to power cycle it and wait another
60 seconds.  Additionally, if you have the dual model, it may be useful for testing purposes to dynamically start and 
stop the two transmitters independently.  With the scan feature, you can also search for nearby beacons and see their identifiers on your display.  In order to control the beacon manually, you will need basic Linux command 
line skills, an ethernet cable, and access to a router.

Step 1. Using an ethernet cable, plug one end into the ethernet port on the Raspberry Pi computer and the other end 
into your router.

Step 2. Power on the Beacon Development Kit.

Step 3. Using a computer, go to your router’s administration page and determine the IP address of the Beacon 
Development Kit.  It should show up on your router’s attached devices list as “Raspberry Pi”

Step 4. On your computer connected to the same router, open up a ssh client and connect to the Beacon Development
Kit, logging in with username: pi, password: raspberry

Step 5. In the console, use the following commands to control the beacon:

```
ibeacon scan     # scans for other nearby beacons 
 ibeacon start    # starts all beacons
 ibeacon stop     # stops all beacons
 ibeacon start 1  # starts the first beacon
 ibeacon stop 1   # stops the first beacon
 ibeacon start 2  # starts the second beacon (dual model only)
 ibeacon stop 2   # stops the second beacon (dual model only)
 ibeacon help     # prints out summary of the different beacon commands
```

You may also adjust the identifiers through the console by editing the /boot/ibeacon.conf file.  If you change the 
identifiers, you will need to rerun the start command in order for the changes to take effect. 

##Beacon Scanning

Visit our [developer blog](http://developer.radiusnetworks.com/blog/) for more information on the scanning feature, including a [quick tutorial](http://developer.radiusnetworks.com/2014/04/27/how-to-make-a-raspberry-pi-turn-on-a-lamp-with-an-ibeacon.html) that takes advantage of the Raspberry Pi output pins to control external devices based on the proximity of nearby beacons.

##Getting Help 

If you have problems configuring or operating the Beacon Development Kit, email us at support@radiusnetworks.com

##About Radius Networks

Radius Networks is a proximity services company that provides solutions to help businesses and individuals enhance 
their experience through mobile device detection and location awareness. Located in Washington DC, Radius was founded 
in 2011 by experienced entrepreneurs to build applications and services around wireless technology and mobile devices. 
Visit our [main website](http://www.radiusnetworks.com) to learn more about what Radius Networks services can do for you.

