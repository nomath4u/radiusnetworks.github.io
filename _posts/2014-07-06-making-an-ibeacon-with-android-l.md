---
author: David G. Young and Matt Tyler
layout: post
title: Making an iBeacon with Android L
---

Our last blog post showed how you could access hidden APIs in a rooted Android 4.4.3 device to make an iBeacon Transmitter.  Now we can demonstrate this same functionality -- and more -- with public APIs inside the Android L operating system, which is expected to be released this fall.

A preview version of Google’s latest operating system for the Android platform was released during Google I/O a just over a week ago.  Developers can flash this bleeding-edge operating system onto Nexus 5 and Nexus 7 phones using images [downloadable from Google.](https://developer.android.com/preview/setup-sdk.html)

Our demo app for Android L looks pretty much like the one we built for Android 4.4.3, except it has Android L-style icons on the bottom of the screen.  Its iBeacon transmissions are detected by both iOS and Android.  And unlike iOS transmitter apps, it continues transmitting even when it is in the background.  You can [download the app for free in the Google Play Store](https://play.google.com/store/apps/details?id=com.radiusnetworks.beacontransmitter), but it will only work if you have Google L installed on your device.

<img style="margin:10px; height: 400px; float:middle;" src='/img/android-l-transmitter.jpg'>

##Good News##

There are several items of good news in Android L when it comes to iBeacons and Bluetooth LE:

* A bug in Android 4.4.3’s Bluetooth Advertising code is no longer an issue -- advertisements in Android L may now be the full 25 bytes necessary to act as a standard iBeacon.  This allows this OS version to include a transmitter power calibration value, allowing for distance estimates between the Android device and an iBeacon.

* The APIs needed to transmit as an iBeacon are public in the Android L preview.  Third parties can develop such apps and install them to the phone just like any other app.  Of course, until this operating system is released, this doesn’t much matter -- phone users need root access to install Android L anyway.  But this confirms that non-rooted Android devices will be able to run third-party apps transmitting as an iBeacon by the end of the year.

* The APIs allow you to select between four different levels of transmitter power.   This feature is important because it allows the beacon to trigger an action at a variable distance (when it is first detected), especially on iOS devices which generally cannot range for iBeacons in the background.  Testing these different settings showed they do indeed make a difference on a Nexus 5.  An iPhone 4S picked up the Android transmission at the following power levels when positioned one meter away.  Note that the ultra low setting is indeed ultra low -- the iPhone could not even pick up the transmission when sitting immediately next to the Nexus 5, although a Mac running ScanBeacon could.  (Presumably the mac has a better antenna than the iPhone 4S for picking up Bluetooth signals.)  Also, note that the high power setting is roughly equivalent to the non-configurable transmitter power of an iPhone, which is -59 dBm at one meter away.

<style type="text/css">
  table.rsum {
    border-collapse: collapse;
    border: 1px solid black;
  }
  table.rsum td{
    border: 1px solid black;
    padding: 5px;
  }
  table.rsum th{
    border: 1px solid black;
    padding: 5px;
  }

</style>

<table class="rsum">
<tr><th>Setting</th><th>RSSI @1m (iOS)</th><th>RSSI @1m (Mac)</th></tr>
<tr><td>ADVERTISE_TX_POWER_HIGH
</td><td>-56 dBm
</td><td>-55 dBm
</td></tr>
<tr><td>ADVERTISE_TX_POWER_MEDIUM
</td><td>-66 dBm
</td><td>-66 dBm
</td></tr>
<tr><td>ADVERTISE_TX_POWER_LOW
</td><td>-75 dBm
</td><td>-70 dBm
</td></tr>
<tr><td>ADVERTISE_TX_POWER_ULTRA_LOW
</td><td>(not detected)
</td><td>-79 dBm
</td></tr>
</table>

* Another setting allows you to configure the transmitter frequency by choosing one of three settings.  This is important because the more frequent the transmissions, the more accurate the distance estimates can be for an Android or iOS device detecting the beacon.  The settings below correspond to the following frequencies.  The frequencies were measured by counting packets detected by a Mac Bluetooth scanner, so they may not be exact.  Note that the `ADVERTISE_MODE_LOW_POWER` setting actually transmits the most frequently -- something that should use more power (possibly signifying a bug in the naming of the settings).

<table class="rsum">
<tr><th>Setting
</th><th>Transmit Frequency
</th></tr><tr><td>ADVERTISE_MODE_LOW_LATENCY
</td><td>approx 1 Hz
</td></tr><tr><td>ADVERTISE_MODE_BALANCED
</td><td>approx 3 Hz
</td><td>ADVERTISE_MODE_LOW_POWER
</td><td>approx 10 Hz
</td></tr></table>

##API Changes

All these changes are possible because Android now has brand new APIs for  interacting with Bluetooth LE under the android.bluetooth.le package documented [here](https://developer.android.com/preview/api-overview.html).
To some extent, these appear to wrap the same Bluedroid drivers under the hood, but the changes are significant.  Source code for Nexus device configurations was released a few days ago, but the more important source code for the APIs is still not available, nor is full documentation.   

But based on what we can derive from interacting with the the binary preview SDK, a new Java API layer for interacting with bluetooth LE has little resemblance to what was in Android 4.4.3.  Some of the hidden APIs needed to transmit as a bluetooth LE peripheral have been moved to the android.bluetooth.le package, and lots of new APIs have been added.  The old public APIs are still available, but the new APIs offer much more functionality.


The code needed to transmit as an iBeacon with Android L’s android.bluetooth.le APIs is a bit different from the way we did it with the android.bluetooth APIs in Android 4.4.3.   The first difference to note is that you no longer need the special android.permission.BLUETOOTH_PRIVILEGED permission to transmit an advertisement -- just  the standard android.permission.BLUETOOTH_ADMIN permission in your AndroidManifest.xml.

Setting up the advertising bytes is pretty much the same with Android L as with Android 4.4.3.  The only difference is that you can have a full 25-byte iBeacon advertisment that includes the calibrated transmitter power.  (Android 4.4.3 forced us to leave off this byte because of a bug limiting advertisement size to 24 bytes.)  For transmitting as an iBeacon, we will need to send out an advertisement that includes the following:

```java
byte[] advertisingBytes;
advertisingBytes = new byte[] { 
(byte) 0x4c, (byte) 0x00,   // Apple manufacturer ID
(byte) 0x02, (byte) 0x15,   // iBeacon advertisement identifier
// 16-byte Proximity UUID follows  
(byte) 0x2F, (byte) 0x23, (byte) 0x44, (byte) 0x54, (byte) 0xCF, (byte) 0x6D, (byte) 0x4a, (byte) 0x0F,
(byte) 0xAD, (byte) 0xF2, (byte) 0xF4, (byte) 0x91, (byte) 0x1B, (byte) 0xA9, (byte) 0xFF, (byte) 0xA6,
(byte) 0x00, (byte) 0x01,   // major: 1
(byte) 0x00, (byte) 0x02,
(byte) 0xC5 }; // transmitter power calibration value: -59 
```

In order to tell Android to transmit this byte sequence, you have to first get an instance of the BluetoothAdapter, and then use the brand new BluetoothLEAdvertiser class which gives you control of advertising.

```java
BluetoothManager bluetoothManager = (BluetoothManager) this.getApplicationContext().getSystemService(Context.BLUETOOTH_SERVICE);
BluetoothAdapter bluetoothAdapter = bluetoothManager.getAdapter();		
BluetoothLEAdvertiser bluetoothAdvertiser = bluetoothAdapter.getBluetoothLeAdvertiser()
```

We next need to set the advertisingBytes on the bluetoothAdvertiser.  The new APIs still allow you to set both ManufacturerData (which will be sent out with Bluetooth AD type 0xFF) and ServiceData (which will be sent out with Bluetooth AD type 0x16)  For the purposes of transmitting as an iBeacon, we must use the ManufacturerData, because iOS devices looking for iBeacons expect to see the 0xFF Bluetooth AD type.

```java
AdvertisementData.Builder dataBuilder = new AdvertisementData.Builder();
dataBuilder.setManufacturerData((int) 0, advertisingBytes);
```

Next we can exercise the additional control Android L gives you over advertising.  Both the transmitter power level and the frequency (“advertiseMode”) can be set:

```java
AdvertiseSettings.Builder settingsBuilder = new AdvertiseSettings.Builder();
settingsBuilder.setAdvertiseMode(AdvertiseSettings.ADVERTISE_MODE_BALANCED);			
settingsBuilder.setTxPowerLevel(AdvertiseSettings.ADVERTISE_TX_POWER_HIGH); 		
settingsBuilder.setType(AdvertiseSettings.ADVERTISE_TYPE_NON_CONNECTABLE);
```

The last step is to start advertising:

```java
bluetoothAdvertiser.startAdvertising(settingsBuilder.build(), dataBuilder.build(), advertiseCallback);
```

You’ll note that this requires an advertiseCallback definition, which we will define the same way as with Android 4.4.3 like this:

```java
private AdvertiseCallback advertiseCallback = new AdvertiseCallback() {

	@Override
	public void onAdvertiseStart(int result) {
		if (result == BluetoothAdapter.ADVERTISE_CALLBACK_SUCCESS) {
			Log.d(TAG, "started advertising successfully.");					
		}
		else {
			Log.d(TAG, "did not start advertising successfully");
		}
		
	}

	@Override
	public void onAdvertiseStop(int result) {
		if (result == BluetoothAdapter.ADVERTISE_CALLBACK_SUCCESS) {
			Log.d(TAG, "stopped advertising successfully");
		}
		else {
			Log.d(TAG, "did not stop advertising successfully");
		}
		
	}
    
};
```

##Putting it all Together##

That’s it!  Execute this code, and your Android L device will start transmitting.  

The transmissions can be picked up on any iPhone or Android device running our Android iBeacon Library, and unlike our version for Android 4.4.3, the distance estimates work, too.   Now we just have to wait for Android L to get in everybody’s hands.
