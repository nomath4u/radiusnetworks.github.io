---
layout: android-ibeacon-library
---

###Configuring the Android iBeacon Library

Note:  if you are configuring the pro version of the library, [instructions are availble here](proximitykit.com/android-download).  [Also, see additional Android Studio troubleshooting info.](/ibeacon/android/pro/android-studio-troubleshooting.html)


#### Eclipse 

1. Download the [tar.gz file](/ibeacon/android/download.html)
2. Extract the above file
3. Import the AndroidIBeaconLibrary as an existing project in the workspace
4. In a new/existing Android Application project, go to Project -> Properties -> Android -> Library -> Add, then select the imported project from step 3.
5. Add the follwoing sdk and permission declarations to your AndroidManifest.xml

   ```
   <uses-sdk
        android:minSdkVersion="18"
        android:targetSdkVersion="18" />
	 <uses-permission android:name="android.permission.BLUETOOTH"/>
	 <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
   ```

6. Edit your project.properties file and add the line: 
   ```
     manifestmerger.enabled=true
   ```


7. <b>Only perform this step if you are using a version prior to 0.7.1 or if you do not have manifest merging enabled per step 6.</b>  If  manifest merging does not work, you must manually add the following service declarations to your AnroidManifest.xml, replacing {my app's package name} with the fully qualified package name of your Android application.


   ```
		<service android:enabled="true"
         	android:exported="true"
         	android:isolatedProcess="false"
         	android:label="iBeacon"
         	android:name="com.radiusnetworks.ibeacon.service.IBeaconService">
		</service>    
		<service android:enabled="true" 
         	android:name="com.radiusnetworks.ibeacon.IBeaconIntentProcessor">
         		<meta-data android:name="background" android:value="true" />
			<intent-filter 
               android:priority="1" >
				<action android:name="{my app's package name}.DID_RANGING" />
				<action android:name="{my app's package name}.DID_MONITORING" />
			</intent-filter>
		</service>  
   ```

#### Android Studio / Gradle 

1. Download the [AAR file](/ibeacon/android/download.html)
2. Create a /libs directory inside your project and copy the AAR file there.
3. Edit your build.gradle file, and add a "flatDir" entry to your repositories like so:

   ```
   repositories {
     mavenCentral()
     flatDir {
       dirs 'libs'
     }
   }
   ```

4. Edit your build.gradle file to add this AAR as a dependency like so:

   ```
   dependencies {
     compile 'com.radiusnetworks:AndroidIBeaconLibrary:0.7.1@aar'
   }
   ```

