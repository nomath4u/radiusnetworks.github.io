---
layout: android-ibeacon-library
---

## Reference Application

A minimalist [reference application](https://github.com/RadiusNetworks/android-ibeacon-reference) for the Open Source Library is available on GitHub that demonstrates basic ranging and monitoring.  It is based on the examples below.

Additional features of the Pro Library are demonstrated in a separate [pro reference application](https://github.com/RadiusNetworks/android-proximity-reference).



## Monitoring Example Code

```
public class MonitoringActivity extends Activity implements IBeaconConsumer {
	protected static final String TAG = "RangingActivity";
	private IBeaconManager iBeaconManager = IBeaconManager.getInstanceForApplication(this);

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_ranging);
		iBeaconManager.bind(this);
	}
	@Override 
	protected void onDestroy() {
		super.onDestroy();
		iBeaconManager.unBind(this);
	}
	@Override
	public void onIBeaconServiceConnect() {
		iBeaconManager.setMonitorNotifier(new MonitorNotifier() {
      	@Override
      	public void didEnterRegion(Region region) {
  	  	  Log.i(TAG, "I just saw an iBeacon for the firt time!");		
      	}

      	@Override
      	public void didExitRegion(Region region) {
          Log.i(TAG, "I no longer see an iBeacon");
      	}

      	@Override
      	public void didDetermineStateForRegion(int state, Region region) {
      		Log.i(TAG, "I have just switched from seeing/not seeing iBeacons: "+state);		
      	}
		});
		
		try {
			iBeaconManager.startMonitoringBeaconsInRegion(new Region("myMonitoringUniqueId", null, null, null));
		} catch (RemoteException e) {	}
	}

}

```


## Ranging Example Code

```
public class RangingActivity extends Activity implements IBeaconConsumer {
	protected static final String TAG = "RangingActivity";
	private IBeaconManager iBeaconManager = IBeaconManager.getInstanceForApplication(this);

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_ranging);
		iBeaconManager.bind(this);
	}
	@Override 
	protected void onDestroy() {
		super.onDestroy();
		iBeaconManager.unBind(this);
	}
	@Override
	public void onIBeaconServiceConnect() {
		iBeaconManager.setRangeNotifier(new RangeNotifier() {
      	@Override 
      	public void didRangeBeaconsInRegion(Collection<IBeacon> iBeacons, Region region) {
      		if (iBeacons.size() > 0) {
	      		Log.i(TAG, "The first iBeacon I see is about "+iBeacons.iterator().next().getAccuracy()+" meters away.");		
      		}
      	}
		});
		
		try {
			iBeaconManager.startRangingBeaconsInRegion(new Region("myRangingUniqueId", null, null, null));
		} catch (RemoteException e) {	}
	}
}

```

## Background Launching Example Code (Pro Library Only)

```
public class AndroidProximityReferenceApplication extends Application implements BootstrapNotifier {
    private static final String TAG = "AndroidProximityReferenceApplication";
    private RegionBootstrap regionBootstrap;


    public void onCreate() {
        super.onCreate();
        Region region = new Region("com.radiusnetworks.androidproximityreference.backgroundRegion",
                "2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6", null, null);
        regionBootstrap = new RegionBootstrap(this, region);
    }

    @Override
    public void didDetermineStateForRegion(int arg0, Region arg1) {
        // This method is not used in this example
    }

    @Override
    public void didEnterRegion(Region arg0) {
        Log.d(TAG, "did enter region.");
        Intent intent = new Intent(this, MainActivity.class);
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);

        // Important:  make sure to add android:launchMode="singleInstance" in the manifest
        // to keep multiple copies of this activity from getting created if the user has
        // already manually launched the app.
        this.startActivity(intent);
    }

    @Override
    public void didExitRegion(Region arg0) {
        Log.d(TAG, "exited region");
    }

}
```

## Auto Battery Saving Example Code (Pro Library Only)

```
public class AndroidProximityReferenceApplication extends Application implements BootstrapNotifier {
    private BackgroundPowerSaver backgroundPowerSaver;

    public void onCreate() {
        super.onCreate();
        // Simply constructing this class and holding a reference to it in your custom Application class
        // enables auto battery saving of about 60%
        backgroundPowerSaver = new BackgroundPowerSaver(this);
    }
}
```

## Accessing iBeacon Data Example Code (Pro Library Only)

<i>Important:  when using the Pro Library, be sure to use com.radiusnetworks.proximity.ibeacon.IBeaconManager instead of com.radiusnetworks.ibeacon.IBeaconManager, or you will get a DataProviderException.</i>

```
import com.radiusnetworks.proximity.ibeacon.IBeaconManager;

public class MainActivity extends Activity implements IBeaconConsumer, RangeNotifier, IBeaconDataNotifier
{
    public static final String TAG = "MainActivity";

    IBeaconManager iBeaconManager;
    Map<String,TableRow> rowMap = new HashMap<String,TableRow>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        iBeaconManager = IBeaconManager.getInstanceForApplication(this.getApplicationContext());
        iBeaconManager.bind(this);
    }

    @Override
    public void onIBeaconServiceConnect() {
        Region region = new Region("MainActivityRanging", null, null, null);
        try {
            iBeaconManager.startRangingBeaconsInRegion(region);
            iBeaconManager.setRangeNotifier(this);
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        iBeaconManager.unBind(this);
    }

    @Override
    public void didRangeBeaconsInRegion(Collection<IBeacon> iBeacons, Region region) {
        for (IBeacon iBeacon: iBeacons) {
            iBeacon.requestData(this);
            String displayString = iBeacon.getProximityUuid()+" "+iBeacon.getMajor()+" "+iBeacon.getMinor()+"\n";
            displayTableRow(iBeacon, displayString, false);
        }
    }

    @Override
    public void iBeaconDataUpdate(IBeacon iBeacon, IBeaconData iBeaconData, DataProviderException e) {
        if (e != null) {
            Log.d(TAG, "data fetch error:"+e);
        }
        if (iBeaconData != null) {
            String displayString = iBeacon.getProximityUuid()+" "+iBeacon.getMajor()+" "+iBeacon.getMinor()+"\n"+"Welcome message:"+iBeaconData.get("welcomeMessage");
            displayTableRow(iBeacon, displayString, true);
        }
    }
    
...    

}
```
