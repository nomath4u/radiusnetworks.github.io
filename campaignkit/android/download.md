
# Download Campaign Kit Library for Android Client

Requirements for use: 

* Android API Level 18 or higher

* Google Play Services library set as a dependency if using Geofencing features.

* CampaignKit.properties file downloaded from campaignkit.radiusnetworks.com.





##If Using Android Studio (we recommend Android Studio v0.8.6 with gradle 1.12):


1) Download the latest [.aar library file](https://github.com/RadiusNetworks/campaignkit-android/releases)
 

2) Add the library as a dependency to your project

 * Drag and drop the .aar file into your Android Studio Project's "libs" folder. This should be next to your "src" folder. If there is no "libs" folder, create one. 

 * Next to your "src" and "libs" folder, there should be a build.gradle file. Open that and include the following:

```java
  dependencies {
      compile fileTree(dir: 'libs', include: ['*.jar'])
      compile 'com.radiusnetworks:campaignkit-android:0+@aar'
  }

  repositories {
      mavenCentral()
      flatDir {
          dirs 'libs'
      }
  }
```


3) Insert CampaignKit.properties file

 * Log into campaignkit.radiusnetworks.com.

 * Select a kit from the drop-down menu. Create one and add in some content, places, and campaigns.

 * On the kit overview page, click the android button in the top right corner. This will download your CampaignKit.properties file.

 * Copy the CampaignKit.properties file into the /src/main/resources folder of your project. Most likely, you will have to create this folder yourself. Please note that this is NOT to be confused with the /src/main/res folder.


4) Adjust Build Settings

 * In the main folder of your project, you will find a project.properites file. On a new line, add in "manifestmerger.enabled=true".

 * In AndroidManifest.xml, set minSdkVersion = 18 and set your targetSdkVersion to 18 or higher. Here's a sample of what that should look like:

```xml
     <uses-sdk
        android:minSdkVersion="18"
        android:targetSdkVersion="19" />
```
 * Adjust your minSdkVersion, targetSdkVersion, compileSdkVersion and buildToolsVersion within the build.gradle file of your project to match what you have in AndroidManifest.xml. Here's a sample that will match our AndroidManifest.xml sample listed above.

```java
android {
    compileSdkVersion 19
    buildToolsVersion '19.1.0'

    defaultConfig {
        applicationId "com.radiusnetworks.campaignkitreference"
        minSdkVersion 18
        targetSdkVersion 19
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            runProguard false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
```


5)  On the top bar of Android Studio click the "Build" drop down, and select "Rebuild project".






##If Using the Eclipse IDE


1) Download the latest [.tar.gz library file](https://github.com/RadiusNetworks/campaignkit-android/releases) Campaign Kit Library for Android Client.
 

2) Add the library as a dependency to your project

 * Double click the .tar.gz file, this will produce a folder containing the library.

 * Import the library as an Android Project to your workspace.
![alt tag](https://raw.githubusercontent.com/RadiusNetworks/campaignkit-documentation/master/docs/android/screenshots/importing.png) 

 * Right-click the library and open the Properties menu.

 * Select the Android tab on the left side of the window.

 * Make sure the library has the "is Library" checkbox checked.
![alt tag](https://raw.githubusercontent.com/RadiusNetworks/campaignkit-documentation/master/docs/android/screenshots/cklibrary_islibrary.png) 

 * Right-click your project and open the Properties menu.

 * Select the Android tab on the left side of the window.

 * Add the campaignkit-android as a library.
![alt tag](https://raw.githubusercontent.com/RadiusNetworks/campaignkit-documentation/master/docs/android/screenshots/adding_cklibrary.png) 

 * Afterwards, your project's Android properties should look like this:
![alt tag](https://raw.githubusercontent.com/RadiusNetworks/campaignkit-documentation/master/docs/android/screenshots/project_settings.png)


3) Insert CampaignKit.properties file

 * Log into campaignkit.radiusnetworks.com.

 * Select a kit from the drop-down menu. Create one and add in some content, places, and campaigns.

 * On the kit overview page, click the Android Button in the top right corner. This will download your CampaignKit.properties file.

 * Copy the CampaignKit.properties file into the /src folder of your project.


4) Adjust Build Settings

 * In the main folder of your project, you will find a project.properties file. On a new line, add in "manifestmerger.enabled=true".

 * In AndroidManifest.xml, set minSdkVersion = 18 and set your targetSdkVersion to 18 or higher. Here's a sample of what that should look like:

```xml
     <uses-sdk
        android:minSdkVersion="18"
        android:targetSdkVersion="19" />
```

5)  On the top bar of Eclipse click the "Project" drop down, and select "Clean..." for all projects.

