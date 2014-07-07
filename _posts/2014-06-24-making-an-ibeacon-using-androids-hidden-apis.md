---
author: David G. Young
layout: post
title: Making an iBeacon with Android's hidden APIs
---

**UPDATE: The new Android L operating system allows you to [make an even better iBeacon transmitter](2014-07-06-making-an-ibeacon-with-android-l.html).**

Since Android added Bluetooth LE support in last year's 4.3 release, the lack of peripheral mode support has been driving many developers crazy.   Without peripheral mode, Android devices can't advertise Bluetooth LE services to other devices.  And while they can detect iBeacons around them, they can't act as iBeacons, because doing so requires advertising as a peripheral. 

This has left some developers sitting on the edge of their seats waiting for the next Android release, hoping that peripheral mode will be added to Android 4.5 — perhaps as soon as the Google I/O conference this week.

But what if I told you that you don't have to wait?  The Android phone you have in your pocket right now might already have peripheral mode support.  If you have a phone running Android 4.4.3, this is absolutely true.  APIs providing peripheral mode support have already been merged over from the Android Open Source Project (AOSP) and sit quietly hidden away inside your phone.  Using them requires nothing more than rooting your phone and installing an app to a special location.

To demonstrate this, I built a demo iBeacon transmitter app for Android, then installed it on my Nexus 5 running Android 4.4.3.  You can see the beacon transmission get picked up by an iPhone 4S by its side.

<img style="margin:10px; height: 400px; float:middle;" src='/img/android-transmitter.jpg'>


##How This Works

The APIs needed to do this are published on the AOSP repo in the [BluetoothAdapter](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/bluetooth/BluetoothAdapter.java) class.    Note the methods startAdvertising(), stopAdvertising() and getAdvScanData(), which allows you to read and write the raw information sent out in the advertisement.   As it stands, these API calls are blocked from use unless an app has android.permission.BLUETOOTH_PRIVILEGED.  This is a system-level permission, so the only way to get this is for your custom app is to root your phone, and install your app in the /system/priv-app directory. 

If you want to develop against these APIs, then you have to take another step, too.  The method calls mentioned above are all marked with an @hide annotation in the Android source, which causes them to be kept out the standard SDK file that you download to build apps against API Level 19.  To get around this, you have to compile the SDK from the AOSP source (after removing these annotations from BluetoothAdapter.java,  BluetoothAdvScanData.java, and running make `update-api`.  Once you build a new SDK with these APIs no longer hidden, you can use this SDK in Eclipse or AndroidStudio to make a custom app that acts as a Bluetooth LE peripheral.

##Demonstrating the Code

An iBeacon is a great example of such an app.  To transmit as a peripheral, you first need to verify that your app has the android.permission.BLUETOOTH_PRIVILEGED permission otherwise none of this will work.  Again, to get this permission, you must put the APK file in the /system/priv-app directory:

```java
if ((this.getApplicationInfo().flags & ApplicationInfo.FLAG_SYSTEM) != 0) {
	Log.d(TAG, "This is a system application");
} 
else {
	Log.d(TAG, "This is not a system application");
}
if (getApplicationContext().checkCallingOrSelfPermission(
	"android.permission.BLUETOOTH_PRIVILEGED") == PackageManager.PERMISSION_GRANTED) {
	Log.d(TAG, "I have android.permission.BLUETOOTH_PRIVILEGED");
} 
else {
	Log.e(TAG, "I do not have android.permission.BLUETOOTH_PRIVILEGED");
	final AlertDialog.Builder builder = new AlertDialog.Builder(this);
	builder.setTitle("Missing BLUETOOTH_PRIVILEGED permission");
	builder.setMessage("This device must be rooted and this app installed in the /system/priv-app directory.");
	builder.setPositiveButton(android.R.string.ok, null);
	builder.setOnDismissListener(new DialogInterface.OnDismissListener() {
		@Override
		public void onDismiss(DialogInterface dialog) {
			finish();
		}
	});
	builder.show();
}
```

Once you have the proper permissions, you simply have to set up your advertisement data and start advertising.  For transmitting as an iBeacon, we will need to send out an advertisement that includes the following:

```java
byte[] advertisingBytes;
advertisingBytes = new byte[] { 
(byte) 0x4c, (byte) 0x00,   // Apple manufacturer ID
(byte) 0x02, (byte) 0x15,   // iBeacon advertisement identifier
// 16-byte Proximity UUID follows  
(byte) 0x2F, (byte) 0x23, (byte) 0x44, (byte) 0x54, (byte) 0xCF, (byte) 0x6D, (byte) 0x4a, (byte) 0x0F,
(byte) 0xAD, (byte) 0xF2, (byte) 0xF4, (byte) 0x91, (byte) 0x1B, (byte) 0xA9, (byte) 0xFF, (byte) 0xA6,
(byte) 0x00, (byte) 0x01,   // major: 1
(byte) 0x00, (byte) 0x02 }; // minor: 2
```

In order to tell Android to transmit this byte sequence, you have to first get an instance of the BluetoothAdapter, and get the current value of the BluetoothAdvScanData, which can be used to set the advertisement bytes: 

```java
BluetoothManagerbluetoothManager = (BluetoothManager) this.getApplicationContext().getSystemService(Context.BLUETOOTH_SERVICE);
BluetoothAdapter bluetoothAdapter = bluetoothManager.getAdapter();
BluetoothAdvScanData scanData = bluetoothAdapter.getAdvScanData();
```
Once you have this scan data, you need to set these bytes.  The Android APIs allow you to set both ManufacturerData (which will be sent out with Bluetooth AD type 0xFF) and ServiceData (which will be sent out with Bluetooth AD type 0x16)  For the purposes of transmitting as an iBeacon, we must use the ManufacturerData, because iOS devices looking for iBeacons expect to see the 0xFF Bluetooth AD type.

```java
scanData.removeManufacturerCodeAndData(0x01);
scanData.setManufacturerData((int) 0x01, advertisingBytes);
scanData.setServiceData(new byte[]{});	// clear out service data.  
```

The last step is to start advertising:

```java
bluetoothAdapter.startAdvertising(advertiseCallback);	
```

You'll note that this requires an advertiseCallback definition, which we will define like this:

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

With all that compiled and installed to the /system/priv-app directory of a rooted phone, the advertisement is properly detected by both by iOS devices and Android devices running an app based on the Android iBeacon Library.  If you have a rooted phone with the latest Android version, you can try it out yourself with the APK from [here](https://account.radiusnetworks.com/orders/new?sku=26).  Instead of downloading this file directly on your Android device, you should download it on your computer and sideload it to a special directory
using the steps below.  **WARNING:  Only perform these steps if you know what you are doing.  Accidentally deleting files in the /system directory on your Android device may make it non-functional.  Proceed at your own risk!**

```
$ adb push AndroidBeaconTransmitter.apk /sdcard/
$ adb shell
$$ su
$$ mount -o remount,rw -t yaffs2 /dev/block/mtdblock3 /system
$$ cp /sdcard/AndroidBeaconTransmitter.apk /system/priv-app/
$$ chmod 644 /system/priv-app/AndroidBeaconTransmitter.apk 
$$ mount -o remount,ro -t yaffs2 /dev/block/mtdblock3 /system
$$ reboot
```

##One More Catch

Astute observers who are in-the-know about iBeacons might notice one more catch:  the advertisement above is missing the 25th byte of the standard iBeacon transmission.  This byte is used to send the calibration power value for the Bluetooth transmitter.  Unfortunately, due to a [bug in Android's GattService.java file](https://code.google.com/p/android/issues/detail?id=72304), manufacturer advertisements are currently limited to 24 bytes instead of the 26 bytes that should be allowed.  Hopefully, this bug will be fixed in the next Android version.  Until then, you can't transmit this power calibration value, meaning that iBeacon detectors won't be able to estimate the distance to an Android iBeacon.  Note in the screenshot that no “accuracy” distance estimate is provided.

##Phones Without Root

But what about folks without rooted phones?  Phone makers like Motorola, Samsung or LG can install apps in the /system/priv-app directory allowing them to use these APIs right away.  For those of us building third-party apps, the only way to use these APIs on a non-rooted device is to wait for a new Android release.  Blocking APIs with @hide annotations is a common way for the Android development team to block new APIs until they are ready to be put into a planned release.  Google has been tight lipped about whether the next release will be Android 4.5 or 5.0, and whether it will be codenamed Lollipop or something else.  But given that the peripheral mode APIs are already available in stealth mode in 4.4.4, it's a good bet that they will be coming soon.

##Final Thoughts

We here at Radius Networks are excited to see Android catching up with iOS in this area.  Many of our developer-partners and customers have been asking us for this functionality.  Now, we finally have some parity.

Note: iBeacon is an Apple trademark

