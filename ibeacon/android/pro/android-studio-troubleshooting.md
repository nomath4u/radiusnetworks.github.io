---
layout: android-ibeacon-library
---

# Android Studio Troubleshooting

Android Studio is supposed to set the Idea configuration from the build.gradle file.  Sometimes, however, these get out of sync and you
have to fix them manually.  If you can build your APK from the command line by typing "gradle build", but the IDE still
shows classes from the AndroidiBeaconLibrary as not existing, you may need to manually edit your app.iml file.

A properly configured app.iml file will include two library entries for the Pro Android iBeacon Library:
  
  * ComRadiusnetworksAndroidProximityLibrary1.aar
  * AndroidIBeaconLibrary
  
If one or both of these are missing from hyour app.iml file, classes will not resolve properly in the IDE.  The bottom
of your app.iml should have entries that look similar to this:

        ...
        <orderEntry type="jdk" jdkName="Android API 18 Platform" jdkType="Android SDK" />
        <orderEntry type="sourceFolder" forTests="false" />
        <orderEntry type="library" exported="" name="android-support-v4" level="project" />
        <orderEntry type="library" exported="" name="ComRadiusnetworksAndroidProximityLibrary1.aar" level="project" />
        <orderEntry type="library" exported="" name="AndroidIBeaconLibrary" level="project" /
      </component>
    </module>
