---
layout: android-ibeacon-library
---

###Alternate Configuration for the Android iBeacon Library

The preferred means of configuring the Android iBeacon Library involves a "library project" that consists of Java jar files and an AndroidManifest.xml file that
gets merged into your project's manifest.  With Eclipse, a library project is set up by importing a new project from a compressed archive.  With
Android Studio, a library project is imported in the form of a .aar file.  In most cases, this is the easiest way to configure the library.  

There are cases, however, where using a library project will not work.  Some build systems like IntelliJ do not support manifest merging.  In such cases, it is 
possible to alternately configure the Android iBeacon Library by extracting the .jar file and the AndroidManifest.xml file from the .aar file and
manually inserting them in your project.

Because there are lots of opportunities for error when performing this alternate configuration, only choose this method if you are confident with configuring classpaths and
AndroidManifest.xml files.

###Extracting the necessary files

1. Download the [AAR file](/ibeacon/android/download.html) for the desired version of the library.
2. Use a zip extractor tool to uncompress the file.
3. Rename the classes.jar file to android-ibeacon-library.jar 
4. Rename the AndroidManifest.xml file to AndroidIBeaconLibraryManifest.xml

###Copying the files into your project

1. Copy the android-ibeacon-library.jar file to a folder where it will be included on the classpath of your project.  (Note:  this folder is specific to the build system you are using.)
2. Open the AndroidIBeaconLibraryManifest.xml file in a text editor, and manually copy the entries into your project's AndroidManifest.xml file.

###Troubleshooting

If you do get errors that library classes are undefined, this means the jar file is not on the classpath.  If you never get a callback to the
`onIBeaconServiceConnected` method, this indicates that the AndroidManifest.xml file was not properly configured.

