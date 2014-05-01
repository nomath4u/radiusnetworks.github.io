---
layout: android-ibeacon-library
---

# Directly Accessing ProximityKit Beacon Data

The example below shows how to Access ProximityKit Data inside your app after a sync completes.  The code allows you 
to get the identifiers for each configured iBeacon, as well as the key/value pairs attached to each.  This will work even
if the major and minor values configured in Proximity Kit are set to null.

Note that as of version 1.1.2.4 of the AndroidProximityLibrary, iBeacon data callbacks for individual detected iBeacons will
have access to the key/value pairs for any Beacon regions defined that match their identifiers, even if the major and/or minor
values are set to null.  For example, if an iBeacon is detected with UUID 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 major 1 and minor 1,
and a ProximityKit beacon region is defined with UUID 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6 major null minor null, the aforementioned
iBeacon will have access to all of the key/value pairs defined for the region.  For more information on how to access beacon data on
iBeacon detection see [here](http://developer.radiusnetworks.com/ibeacon/android/samples.html).


        package com.example;
        
        import android.app.Application;
        import android.util.Log;
        
        import com.radiusnetworks.ibeacon.IBeacon;
        import com.radiusnetworks.ibeacon.IBeaconData;
        import com.radiusnetworks.ibeacon.Region;
        import com.radiusnetworks.ibeacon.client.DataProviderException;
        import com.radiusnetworks.proximity.ProximityKitManager;
        import com.radiusnetworks.proximity.ProximityKitNotifier;
        import com.radiusnetworks.proximity.model.KitIBeacon;
        
        /**
         * Created by dyoung on 5/1/14.
         */
        public class MyApplication extends Application implements ProximityKitNotifier {
        
            private ProximityKitManager manager;
            private static String TAG="MyApplication";
        
            @Override
            public void onCreate() {
                super.onCreate();
                manager = ProximityKitManager.getInstanceForApplication(this);
            }
        
            @Override
            public void didSync() {
                // Called when ProximityKit data are updated from the server
        
                // The loop below will access every beacon configured in the kit, and print out the value
                // of an attribute named "myKey"
                for (KitIBeacon iBeacon : manager.getKit().getIBeacons()) {
                    Log.d(TAG, "For iBeacon: "+iBeacon.getProximityUuid()+" "+iBeacon.getMajor()+" "+iBeacon.getMinor()+
                               ", the value of myKey is "+iBeacon.getAttributes().get("myKey"));
                }
            }
        
            @Override
            public void didFailSync(Exception e) {
        
            }
        
            @Override
            public void iBeaconDataUpdate(IBeacon iBeacon, IBeaconData iBeaconData, DataProviderException e) {
        
            }
        
            @Override
            public void didEnterRegion(Region region) {
        
            }
        
            @Override
            public void didExitRegion(Region region) {
        
            }
        
            @Override
            public void didDetermineStateForRegion(int i, Region region) {
        
            }
        }
