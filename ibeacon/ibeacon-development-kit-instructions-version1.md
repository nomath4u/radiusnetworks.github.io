---
layout: ibeacon
---

#Beacon Development Kit Instructions (Version 1.0)

Note: These instructions are for a previous version of the Beacon Development Kit (shipped Oct. 2013 - Jan. 2014).  For instructions on the latest version [click here](http://developer.radiusnetworks.com/pibeacon/pibeacon-instructions.html). 

### Picking a power supply

The Beacon Development Kit requires a standard 5V power supply with a micro USB connector.  The Raspberry Pi computer can be sensitive to poorly performing chargers.
If your charger works, you should see a red LED illuminated when the board is powered, and a green LED that flashes intermittently.  If this doesn't happen, try a different charger.

### Verifying connections

Verify that the SD and the Bluetooth dongle are properly seated in their sockets, and that the power supply is connected to a wall socket and the micro USB connector on the board.

### Powering on the Beacon

There is no power switch for the beacon.  As soon as the 5V micro USB adapter is connected to a wall socket and the computer board, the beacon will start up.  It takes about 60 seconds for it
to start transmitting.

### Verifying it Works

Use an beacon test program like Radius Networks' free Beacon Locate app for [iOS](http://store.radiusnetworks.com/collections/all/products/locate-ibeacon-app) to verify the beacon is transmitting.  See below for the beacon identifiers.

### Customizing the Beacon identifiers

Beacons have a three part identifier consisting of the ProximityUUID, the major and the minor.  By default, the beacon will transmit with
a ProximityUUID of E2C56DB5-DFFB-48D2-B060-D0F5A71096E0, major of 1 and minor of 1.  This matches the default test identifier of Apple's AirLocate
program, so you can use that to test that it is working.

The ProximityUUID is a 16 byte indentifier, usually expressed as a series of hexadecimal digits separted by dashes.  Generally, you generate one ProximityUUID to use in all of your organization's beacons.  If you don't have one already, you can use
any GUID/UUID generation tool that outputs an identifier that looke the the one above.

The major and minor are both integers that can be between between 0 and 63335.  The major is used to put beacons into a logical group, like a building or a room.  And the minor is used to identify a single beacon.

In order to put your own beacon identifiers into the Beacon Development Kit hardware, you must put them in the proper format, which means hexadecimal bytes spearated by spaces.  For the
Proximity UUID, this is easy.  Just remove any dashes and put a space after every two characters.  For the example above, the proper format is:

```
E2 C5 6D B5 DF FB 48 D2 B0 60 D0 F5 A7 10 96 E0
```

Encoding the major and minor in this format is a little bit harder because they first have to be converted to hex.  You can use any online decimal to hex conversion tool to do this.  But please note that if
your major or minor number in hex is less than four digits, you must zero fill the left-hand side.

For example, to encode a major value of 1225, you would first convert it to hex, which is 4C9.  Because that is only three characters, you put a zero in front like this: 04C9.
Finally, you split it into two character pieces so it looks like this:

```
04 C9
```

Another example, to encode a minor value of 13, you first convert it to hex, which is D.  Then you zero fill the left side to create four characters and get 000D.  Splitting it into two
character segments you get:

```
00 0D
```

Once you have the identifiers in the proper format, you can put them onto the SD card.  The easiest way to do this is with a card reader.  Unplug the beacon, then remove the SD card and put it into your computer's SD card reader.
After attaching it to your computer with a card reader, look for a volume with a file named ibeacon.conf in the root directory.  Open that file and change the identifiers to be
your own, taking care to keep the quotation marks and other parts of the lines unchanged.

```
# All values must be in hex form separated by spaces between every two hex digits
export BLUETOOTH_DEVICE=hci0
export UUID="e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0"
export MAJOR="00 01"
export MINOR="00 01"
export POWER="c9"
```

Be sure to properly save the file and detach the SD card from your system before ejecting the card, otherwise the card may be corrupted.

### Next Steps

Now it's time to start developing your app!  And when you are ready to deploy your iBeacons, contact Radius Networks to buy models suitable for production use.



