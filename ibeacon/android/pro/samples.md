---
layout: android-ibeacon-library
---

## Starting an App in the Background (Pro version only)

The example below shows you how to make an app that launches itself when it first sees an iBeacon region.  In order for this to work, the app must have been launched
by the user at least once.  The app must also be installed in internal memory (not on an SD card.)

You must create a class that extends `Application` (shown in the example) and then declare this in your AndroidManifest.xml.

Here is the AndroidManifest.xml entry.  Note that it declares a custom Application class, and a background launch activity marked as "singleInstance".

```
    <application 
        android:name="com.radiusnetworks.proximityreference.AndroidProximityReference"
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <!-- Note:  the singleInstance below is important to keep two copies of your activity from getting launched on automatic startup -->
        <activity
            android:launchMode="singleInstance"  
            android:name="com.radiusnetworks.proximityreference.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
		<action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        ...

        <service android:enabled="true" android:exported="false"
            android:name="com.radiusnetworks.ibeacon.IBeaconIntentProcessor">
			<meta-data android:name="background" android:value="true" />
            <intent-filter 
               android:priority="1" >
                <action android:name="com.radiusnetworks.proximityreference.DID_RANGING" />
                <action android:name="com.radiusnetworks.proximityreference.DID_MONITORING" />
            </intent-filter>
        </service> 
    </application>
```


And here is an example Application class.  This will launch the MainActivity as soon as any iBeacon is seen.

```
import android.app.Application;
import android.content.Intent;
import android.util.Log;

import com.radiusnetworks.proximity.ibeacon.startup.BootstrapNotifier;
import com.radiusnetworks.proximity.ibeacon.startup.RegionBootstrap;
import com.radiusnetworks.ibeacon.Region;

public class AndroidProximityReference extends Application implements BootstrapNotifier {
	private static final String TAG = "AndroidProximityReference";
	private RegionBootstrap regionBootstrap;

	@Override
	public void onCreate() {
        	super.onCreate();
		Log.d(TAG, "App started up");
		// wake up the app when any ibeacon is seen (you can specify specific uuid/major/minor filers in the parameters below)
        	Region region = new Region("com.example.myapp.boostrapRegion", null, null, null);
        	regionBootstrap = new RegionBootstrap(this, region);
	}

	@Override
	public void didDetermineStateForRegion(int arg0, Region arg1) {
		// Don't care
	}

	@Override
	public void didEnterRegion(Region arg0) {
		Log.d(TAG, "Got a didEnterRegion call");
		// This call to disable will make it so the activity below only gets launched the first time an iBeacon is seen (until the next time the app is launched)
		// if you want the Activity to launch every single time iBeacons come into view, remove this call.  
		regionBootstrap.disable();
		Intent intent = new Intent(this, MainActivity.class);
		// IMPORTANT: in the AndroidManifest.xml definition of this activity, you must set android:launchMode="singleInstance" or you will get two instances
		// created when a user launches the activity manually and it gets launched from here.
		intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
		this.startActivity(intent);
	}

	@Override
	public void didExitRegion(Region arg0) {
		// Don't care
	}    	
}

```
