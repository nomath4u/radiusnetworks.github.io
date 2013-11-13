---
author: David Helms
layout: post
title: How to Make an iBeacon With the TI CC2540
---

We’ve seen a couple of posts on the web with hex files that you can use to flash a TI CC2540-based system and turn it into an iBeacon with a fixed set of attributes, but we haven’t seen anyone describe the process of how to actually create that hex file, so the team at Radius Networks thought we would lay out for you the quick and dirty approach.

###Get the Software and Hardware

Here are the main things your are going to need:

1. A Windows computer.
2. The [IAR Embedded Workbench] (http://www.iar.com/Service-Center/Downloads/), which you can get with a 30-day evaluation license.
3. An mobile device with Bluetooth 4.0 support and an app to test with, like our iBeacon Locate discovery app.
  * For Android, you can get this app from the [Google Play Store.](https://play.google.com/store/apps/details?id=com.radiusnetworks.ibeaconlocate&hl=en)
  * For iPhone, you can get the this app from the [App Store.](https://itunes.apple.com/us/app/ibeacon-locate/id738709014)
4. The [CC2540DK-MINI development kit] (http://www.ti.com/tool/cc2540dk-mini) and supporting software from TI.


After you’ve gotten your hands on a development kit and installed all the software listed above, it’s time to get to work!

Here are the parts from the dev kit that we are going to be using.

<img style="margin-top:-40px; height: 200px; border: thin solid #999; float:right; border-radius: 4px;" src='/img/TI-2.png'>

####Parts List
  * USB Cable for CC Debugger
  * CC Debugger Module
  * Ribbon Cable Adapter for CC Debugger
  * 10 Pin Ribbon Cable
  * CC2540 Mini DK Key Fob Module
  * CR2032 Coin Cell Battery

One thing to note is that the battery that came with this kit was dead. So, if things don’t seem to be working, you may need a replacement battery.

<img style="margin-right: 10px; height: 200px; border: thin solid #999; float:left; border-radius: 4px;" src='/img/TI-3.png'>

Insert the battery and assemble the cables, CC Debugger module and the CC2540 module as shown in the image to the left. Plug the CC Debugger into your Windows computer and check to see if the LED on the CC Debugger turns green. If the LED on the CC Debugger is illuminated RED, press the reset button on the CC Debugger. If that doesn’t work try swapping the orientation of your ribbon cable and resetting. If that doesn’t work try reinserting your battery to make better contact and the ribbon-cable thing and the reset button thing. Lastly, go get yourself a new battery and start all over again.

###Create the iBeacon Project

From installing the software for the development kit, you should have created the `C:\Texas Instruments` directory. This is the directory that contains all the code we need. Make a copy of the directory: `C:\Texas Instruments\BLE-CC254x-1.3.2\Projects\ble\SimpleBLEBroadcaster` and call it to `C:\Texas Instruments\BLE-CC254x-1.3.2\Projects\ble\iBeacon`

Launch the IAR Embedded Workbench IDE and select: `File > Open > Workspace` to open the workspace at `C:\Texas Instruments\BLE-CC254x-1.3.2\Projects\ble\iBeacon\SimpleBLEBroadcaster.`

It will complain that the workspace is an old format, so select `YES` to have it updated for your version of the workbench.

The IDE should look like this when the project is opened.

<img style="margin-right: 10px; width: 500px; border: thin solid #999; border-radius: 4px;" src='/img/IAR_Screenshot.png'>

For simplicity, we are going to work with the CC2540 configuration, but there are other configurations, including one specifically for the additional features of the key fob that you could use. Check out the drop down menu at the top of the Files window to explore those more

###Modify the Code

In the App folder, double-click on the `simpleBLEBroadcaster.c` file to open it in the editing window. Scroll down until you find this section of code.

```
// GAP - Advertisement data (max size = 31 bytes, though this is
// best kept short to conserve power while advertisting)
static uint8 advertData[] =
{
  // Flags; this sets the device to use limited discoverable
  // mode (advertises for 30 seconds at a time) instead of general
  // discoverable mode (advertises indefinitely)
  0x02,   // length of this data
  GAP_ADTYPE_FLAGS,
  GAP_ADTYPE_FLAGS_BREDR_NOT_SUPPORTED,

  // three-byte broadcast of the data "1 2 3"
  0x04,   // length of this data including the data type byte
  GAP_ADTYPE_MANUFACTURER_SPECIFIC,      // manufacturer specific advertisement data type
  1,
  2,
  3
};
```
Let’s change this to get rid of those flags and to specify the advertising data to match the AirLocate example.

* UUID: `E2C56DB5-DFFB-48D2-B060-D0F5A71096E0`
* Major: `1 (0x0001)`
* Minor: `1 (0x0001)`
* Measured Power: `-59 (0xc5)`

Replace that code with this:

```
// GAP - Advertisement data (max size = 31 bytes, though this is
// best kept short to conserve power while advertisting)
static uint8 advertData[] =
{
  // 25 byte ibeacon advertising data
  // Preamble: 0x4c000215
  // UUID: E2C56DB5-DFFB-48D2-B060-D0F5A71096E0
  // Major: 1 (0x0001)
  // Minor: 1 (0x0001)
  // Measured Power: -59 (0xc5)
  0x1A, // length of this data including the data type byte
  GAP_ADTYPE_MANUFACTURER_SPECIFIC, // manufacturer specific advertisement data type
  0x4c,
  0x00,
  0x02,
  0x15,
  0xe2,
  0xc5,
  0x6d,
  0xb5,
  0xdf,
  0xfb,
  0x48,
  0xd2,
  0xb0,
  0x60,
  0xd0,
  0xf5,
  0xa7,
  0x10,
  0x96,
  0xe0,
  0x00,
  0x01,
  0x00,
  0x01,
  0xc5
};
```
Where `0x4c000215` is the iBeacon preamble and the rest of the bytes correspond to the beacon attributes as previously defined.

Next, find this code which defines the advertisement type:

```
//uint8 advType = GAP_ADTYPE_ADV_NONCONN_IND;// use non-connectable advertisements
uint8 advType = GAP_ADTYPE_ADV_DISCOVER_IND; // use scannable unidirected advertisements
```
Change it to non-connectable by uncommenting the first line and commented out the second line.

```
uint8 advType = GAP_ADTYPE_ADV_NONCONN_IND;   // use non-connectable advertisements
//uint8 advType = GAP_ADTYPE_ADV_DISCOVER_IND; // use scannable unidirected advertisements
```
Below this, you’ll find where we set the GAP Role parameters

```
// Set the GAP Role Parameters
GAPRole_SetParameter( GAPROLE_ADVERT_ENABLED, sizeof( uint8 ), &initial_advertising_enable );
GAPRole_SetParameter( GAPROLE_ADVERT_OFF_TIME, sizeof( uint16 ), &gapRole_AdvertOffTime );

GAPRole_SetParameter( GAPROLE_SCAN_RSP_DATA, sizeof ( scanRspData ), scanRspData );
GAPRole_SetParameter( GAPROLE_ADVERT_DATA, sizeof( advertData ), advertData );
GAPRole_SetParameter( GAPROLE_ADV_EVENT_TYPE, sizeof( uint8 ), &advType );
```

Comment out the 2 lines setting the `GAPROLE_ADVERT_OFF_TIME` and the `GAPROLE_SCAN_RSP_DATA` since iBeacons don’t ever stop advertising and iBeacons don’t tell you their name or provide any other response data.

```
// Set the GAP Role Parameters
GAPRole_SetParameter( GAPROLE_ADVERT_ENABLED, sizeof( uint8 ), &initial_advertising_enable );
//GAPRole_SetParameter( GAPROLE_ADVERT_OFF_TIME, sizeof( uint16 ), &gapRole_AdvertOffTime );

//GAPRole_SetParameter( GAPROLE_SCAN_RSP_DATA, sizeof ( scanRspData ), scanRspData );
GAPRole_SetParameter( GAPROLE_ADVERT_DATA, sizeof( advertData ), advertData );
GAPRole_SetParameter( GAPROLE_ADV_EVENT_TYPE, sizeof( uint8 ), &advType );
```
Make sure you save your changes, because as far as code changes go….that’s it!

###Build and Verify the Project

Now, do the following:

1. Select Project > Clean to clean the project.
1. Select Project > Make to make the project.
1. Select Project > Download and Debug to send the code to the CC2540 Key Fob
1. Select Debug > Go to Run the code on the CC2540 Key Fob.

You may see some warnings complaining about SFRs. This is to be expected and has to do with a mismatch in the debugger setup of the version of the workbench you are using and the sample code from TI. Just ignore them. In fact if you try to fix them, you’ll most likely mess up the configuration and have to start from scratch. Just let go and learn to live with uncertainty.

Your CC2540’s LED should have illuminated RED and if you use the AirLocate iPhone app or the iBeacon Locate Android app you should be able to detect these advertisements. If you disconnect the CC2540 key fob from the CC Debugger, it will continue to run until you remove the battery.

<img style="margin-right: 10px; width: 500px; border: thin solid #999; border-radius: 4px;" src='/img/TI-4.png'>

In the Output folder, you will find a file named `SimpleBLEBroadcaster.hex`. This is the hex file containing your iBeacon embedded application. In the future, if you need to, you can flash this file to the CC2540 key fob module directly using the SmartRF Flash Programmer available from TI.

###  Enjoy!

This is obviously quick and dirty, and you’d want to do a lot more cleanup on this code. explore the other configurations, and also apply these steps to the CC2540 USB Dongle and the CC2541 Sensor Tag, both of which can also be iBeacon-ized. But the essentials are all here for getting your CC2540-based iBeacon up and running and we hope it points you in the right direction for your development efforts. And don’t forget, when it comes to writing your proximity-enabled mobile app solutions, [Proximity Kit](http://ProximityKit.com) is the way to go!.

