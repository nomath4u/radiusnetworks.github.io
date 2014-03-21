---
layout: android-ibeacon-library
---

###Centrally Managing iBeacon Data

The Pro Android iBeacon Library automatically provides access to iBeacon-specific data.  For example, you can configure a different welcome message for each iBeacon and display it to a user inside your app.  This message can be later changed in the cloud and deployed apps will receive the update.

####Key benefits

Without a cloud service to manage your iBeacon identifiers and data,  these values must be hard coded in your app.  This can be a real problem if your identifiers or data need to change because a new version of the app must be created and pushed out to users.  By putting this configuration in the cloud, you avoid the need to make frequent app updates and are able to make changes whenever you want.

Best of all, using the included cloud service with the Pro Library allows you all of these advantages without you having to build, deploy and maintain your own web service.

####What kinds of data can be attached to iBeacons?

Pretty much anything.  Examples include latitude/longitude, address info, website URLs, etc.  The data must be simple string key/value pairs, but by using URLs as the value, you can remotely load images and other binary data as well.

####Where are the data stored?

The data are entered and stored on Radius Networks' Proximity Kit cloud service and synchronized to the phone when your app starts up.

####What if there is no network connectivity?

If no network is available, the last cached data will still be available.  When network connectivity resumes, data will update.

####How to set it up

First, log in to www.proximitykit.com using the same username and password used to download the Pro Android iBeacon Library.  For each "kit" (representing a single  mobile app), you can define one or more iBeacons, then define multiple key/value pairs to each iBeacon. 

Once your data are configured, they will be available when your app detects the associated iBeacon.  The calls necessary to access these data are shown in the sample code [here.](
/ibeacon/android/samples.html)

Pro Android iBeacon Library (c) 2013, 2014 Radius Networks, Inc.
