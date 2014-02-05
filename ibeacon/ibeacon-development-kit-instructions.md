---
layout: ibeacon
---

#iBeacon Development Kit Instructions (Version 2.0)

Note: These instructions are for the latest version of the iBeacon Development Kit (shipping as of February, 2014), for instructions from previous versions (Oct. 2013 - Jan. 2014) [click here](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit-instructions-version1.html).

##Getting Started

###Picking a power supply

The iBeacon Development Kit requires a standard 5V power supply with a micro USB connector. The Raspberry Pi computer
can be sensitive to poorly performing chargers. If your charger works, you should see a red LED illuminated when the
board is powered, and a green LED that flashes intermittently. If this doesn't happen, try a different charger.

###Verifying connections

Verify that the SD card and the Bluetooth dongle are properly seated in their sockets, and the power supply is connected
to a wall socket and the micro USB connector on the board.

###Powering on the iBeacon

There is no power switch for the iBeacon Development Kit. As soon as the 5V micro USB adapter is connected to a wall
socket and the computer board, the iBeacon will start up automatically. It takes about 60 seconds for it to begin
transmitting.

###Verifying it Works

Use an iBeacon test program like Radius Networks’ “Locate for iBeacon” (iOS) or “iBeaconLocate” (Android) to verify the
iBeacon is transmitting. [Click here](http://www.radiusnetworks.com/ibeacon-services.html) for information on how to download these free tools. 

###Information on iBeacon identifiers

iBeacons have a three part identifier consisting of ProximityUUID, Major, and Minor. By default, the iBeacon Development
Kit will transmit with a ProximityUUID of 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6, Major of 1, and Minor of 1.  If you have
the Dual iBeacon Development Kit model, it will also transmit as a second iBeacon with 
ProximityUUID of 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6, Major of 1, and Minor of 2.  These values are included in the 
default identifier list in Radius Networks’ iBeacon test apps for iOS and Android so these tools can be used to test 
if the iBeacon is working.

The ProximityUUID is a 16 byte identifier, usually expressed as a series of hexadecimal digits separated by dashes. 
Generally, you generate one ProximityUUID to use in all of your organization's iBeacons. If you don't have one already,
you can use any GUID/UUID generation tool (for example: the uuidgen command in OS X) to create your own identifier
similar to the one listed above.  Radius Networks’ iBeacon test apps (listed above) also have tools to generate your own
UUID.

The Major and Minor are both integers that can be between between 0 and 65535. The Major is used to put iBeacons into a
logical group, like a building or a room. The Minor is used to identify an individual iBeacon within the group.

Power is a measure of signal strength received by a device one meter from the iBeacon.  This is used by another device
to calibrate distance estimates from the iBeacon.  The iBeacon Development Kit comes preconfigured with the default 
value for the included bluetooth adapter (-59) so you only need to change this value if using another adapter.  If you
decide to use another adapter, you can use Radius Networks’ iBeacon test apps to find its calibrated power value.

Broadcast Frequency indicates how often the iBeacon will broadcast its advertisement -- in times per second 
(Max = 10, Min = 0.25).  With this new feature you can select how often the iBeacon broadcasts, which will help you 
determine how your app will respond to iBeacons with different advertising rates.  By default this value is set to 10 
times per second, which is the setting recommended by Apple engineers.

##Customizing the iBeacon identifiers

With the Radius Networks iBeacon Development Kit, you can easily customize the identifiers being broadcast by the 
iBeacon.  The easiest way to do this is with a card reader and a computer (if your computer doesn’t have an SD card 
reader skip to the next section to connect to the Raspberry Pi and edit these values in the console). Unplug the 
iBeacon, then remove the SD card and put it into your computer's SD card reader. After attaching it to your computer, 
look for a volume with a file named ibeacon.conf in the root directory. When you open this file, you should see two 
blocks like the one below, these give the identifiers for each transmitter (Note: changing the second set of identifiers
does nothing unless you have the dual model).

```    
export UUID1=2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6`
 export MAJOR1=1`
 export MINOR1=1`
 export POWER1=-59
 export BROADCAST_FREQUENCY1=10
```

To change iBeacon identifiers simply edit the values for each of these parameters.  Just paste your UUID in the standard
format (both uppercase and lowercase letters are acceptable) and enter the other values in decimal format.  Be sure to 
properly save the file and detach the SD card from your system before ejecting the card, otherwise the card may be 
corrupted.  Note: if you change to a custom UUID you will need to add this UUID to your ‘Locate for iBeacon’ iOS app 
for it to be visible since iOS devices can only detect iBeacons with known UUIDs.

###Next Steps
Now it's time to start developing your app! If you are developing for Android, be sure to check out Radius Networks'
Android iBeacon Library. When you are ready to deploy your iBeacons, visit radiusnetworks.com to buy models suitable
for production use.

##Controlling the iBeacon(s) Manually
While developing, you may find it useful to start and stop the iBeacon without having to power cycle it and wait another
60 seconds.  Additionally, if you have the dual model, it may be useful for testing purposes to dynamically start and 
stop the two transmitters independently.  In order to control the iBeacon manually, you will need basic Linux command 
line skills, an ethernet cable, and access to a router.

Step 1. Using an ethernet cable, plug one end into the ethernet port on the Raspberry Pi computer and the other end 
into your router.

Step 2. Power on the iBeacon Development Kit.

Step 3. Using a computer, go to your router’s administration page and determine the IP address of the iBeacon 
Development Kit.  It should show up on your router’s attached devices list as “Raspberry Pi”

Step 4. On your computer connected to the same router, open up a ssh client and connect to the iBeacon Development
Kit, logging in with username: pi, password: raspberry

Step 5. In the console, use the following commands to control the iBeacon:

```
       ibeacon start    # starts all iBeacons
	ibeacon stop     # stops all iBeacons
	ibeacon start 1  # starts the first iBeacon
	ibeacon stop 1   # stops the first iBeacon
	ibeacon start 2  # starts the second iBeacon (dual model only)
	ibeacon stop 2   # stops the second iBeacon (dual model only)
	ibeacon help     # prints out summary of the different ibeacon commands
```

You may also adjust the identifiers through the console by editing the /boot/ibeacon.conf file.  If you change the 
identifiers, you will need to rerun the start command in order for the changes to take effect.

##Getting Help 
If you have problems configuring or operating the iBeacon Development Kit, email us at support@radiusnetworks.com

##About Radius Networks
Radius Networks is a proximity services company that provides solutions to help businesses and individuals enhance 
their experience through mobile device detection and location awareness. Located in Washington DC, Radius was founded 
in 2011 by experienced entrepreneurs to build applications and services around wireless technology and mobile devices. 
Visit [radiusnetworks.com](www.radiusnetworks.com) to learn more about what Radius Networks services can do for you.


