---
layout: android-ibeacon-library
---

###Configuring the Android iBeacon Pro Library


#### Eclipse 

Step 1. Get the Library and Import it to Eclipse

Download the [tar.gz file](/ibeacon/android/download.html)

Extract the above file

Launch Eclipse and import the AndroidProxomityLibrary folder above as an existing project in the workspace

Step 2. Get the Config File

Click the button below to download the ProximityKit.properites config file 



Step 3. Configure your Eclipse project

Copy the ProximityKit.properties file from your step 2 into the src folder of your project.

Go to Project -> Properties -> Android -> Library -> Add, then select the imported project from step 3.

Add the follwoing sdk and permission declarations to your AndroidManifest.xml

   ```
   <uses-sdk
        android:minSdkVersion="18"
        android:targetSdkVersion="18" />
	 <uses-permission android:name="android.permission.BLUETOOTH"/>
	 <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
   ```

Edit your project.properties file and add the line: 
   ```
     manifestmerger.enabled=true
   ```



#### Android Studio / Gradle 


Step 1. Get the Library and Copy it to Your Project

Download the [AAR file](/ibeacon/android/download.html)

Create a /libs directory inside your project and copy the AAR file there.

Step 2. Get the Config File

Click the button below to download the ProximityKit.properites config file 

Copy this ProximityKit.properties file to the src/main/java folder of your project

Step 3. Configure Your Main build.gradle File

add a "flatDir" entry to your repositories like so:

   ```
   repositories {
     mavenCentral()
     flatDir {
       dirs 'libs'
     }
   }
   ```

add the ProximityLibrary AAR as a dependency like so:

   ```
   dependencies {
     compile 'com.radiusnetworks:AndroidProximityLibrary:1+@aar'
   }
   ```

add a new sourceSets entry under the android block:

android {
    sourceSets {
        main {
            resources.srcDirs = ['src/main/java']
        }
    }
}

   
See [the reference app's build.gradle file](https://github.com/RadiusNetworks/android-proximity-reference/blob/master/AndroidProximityReference/build.gradle) for an example. 

Pro Android iBeacon Library (c) 2013, 2014 Radius Networks, Inc.
