---
layout: android-ibeacon-library
---

## Reference Application

A minimalist [reference application](https://github.com/RadiusNetworks/android-ibeacon-reference) is available on GitHub that demonstrates basic ranging and monitoring.  It is based on the examples below.


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
