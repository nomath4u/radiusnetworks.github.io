---
layout: android-ibeacon-library
---

###Battery manager

The Pro Android iBeacon Library includes a battery manager that automatically saves 60% or more of devices' battery life when your iBeacon-app is running in the background.

####Why do you need this?

iBeacons are Bluetooth LE radio transmitters, and detecting one with your Android device requires doing a Bluetooth LE scan.  While Bluetooth LE is a lower-energy system than traditional Bluetooth, scanning is still a fairly power-intensive operation.  If your app constantly scans for iBeacons, it drains the battery at a similar rate as your cellular radio.  That means unhappy app users with dead batteries.

####How it works

The battery manager automatically slows down the Bluetooth LE scan rate when your app is in the background.  By default, it does a 30 second scan once every five minutes -- similar to the way iOS behaves when an app is not Ranging for iBeacons in the foreground.  But unlike iOS, the details are completely transparent and completely configurable.  You can reduce the background scan frequency further to save even more power, or increase it to make your app more reponsive -- it all depends on your requirements!

####How much does it save?

Our tests show that a Nexus 5 drains the battery at a rate of 90mA when looking for iBeacons in the foreground.  The default settings for the battery manager reduce this drain to 37mA -- about a 60% savings.  But again, you can customize these settings to save even more.

####How do I set this up?

Setting this up is super easy.  Simply create a custom Android Application class and construct the BackgroundPowerSaver class.  Like this:

```
...
import com.radiusnetworks.proximity.ibeacon.powersave.BackgroundPowerSaver;

public class MyApplication extends Application {
    private BackgroundPowerSaver backgroundPowerSaver;

    public void onCreate() {
        super.onCreate();
        backgroundPowerSaver = new BackgroundPowerSaver(this);
    }
}

```

####How do I customize the background scan rate?

You may alter the default background scan period and the time between scans using the methods on the IBeaconManager class:

```
// set the duration of the scan to be 1.1 seconds
iBeaconManager.setBackgroundScanPeriod(1100l); 
// set the time between each scan to be 1 hour (3600 seconds)
iBeaconManager.setBackgroundBetweenScanPeriod(3600000l);
```
