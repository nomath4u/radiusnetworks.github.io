---
layout: post
title: CampaignKit on Android Wearables
author: Christopher Harper
---

Smartwatches have grown in popularity in the past year, originating with the
Pebble smartwatch on Kickstarter. Then Android followed suit over this
summer with Android Wear. These devices with their portable and quick nature
lend themselves well to CampaignKit, displaying a nice little chunk of
information in a manner similar to a push notification. The following 
will show you how to pop campaign information on Android Wear
devices and the Pebble.

It is assumed you are familiar with CampaignKit and Android Studio, and you have an
active campaign to display.

##Setup

First you are going to need the Android Studio CampaignKit reference app provided
[here](https://github.com/RadiusNetworks/campaignkit-reference-android-studio).
Follow the instructions there and make sure that your
campaign triggers in the app before continuing.


##Android Wear

Unless you already have an Android Wear device you are going to need
to set up an emulator to receive the CampaignKit 
message. Documentation for setting up an emulator can be found [here](https://developer.android.com/training/wearables/apps/creating.html).

![CampaignKit on Wear Emulator](../img/wearkit.png)

The following code builds a notification to send to the Android Wear device and
then sends it. This is most naturally placed in your `didFindCampaign(Campaign c)` method.

	Intent viewIntent = new Intent(this, MainActivity.class);
	PendingIntent viewPendingIntent = PendingIntent.getActivity(this, 0, viewIntent, 0);
	NotificationCompat.Builder notificationBuilder = new NotificationCompat.Builder(getApplicationContext())
	        .setSmallIcon(R.drawable.ic_launcher)
	        .setContentTitle(c.getTitle())
	        .setContentText(c.getMessage())
	        .setContentIntent(viewPendingIntent);
	NotificationManagerCompat notificationManager = getSystemService(Context.NOTIFICATION_SERVICE);
	notificationManager.notify(001, notificationBuilder.build());

The `viewIntent` used in `viewPendingIntent` is where you can decide
what kind of interaction you want the user to be able to have with the notification.
Perhaps you would like a promotion to be opened on the phone? In this case we
reopen the reference app. If you simply want the notification to be dismissed
upon reading, the intent is unnecessary.


##Pebble


![CampaignKit on Pebble](../img/pebblekit.png)

Because Pebble is not specific to Android it has a separate framework. More
documentation can be found at developer.getpebble.com. In order to get a 
notification to the Pebble we need the following.

In build.gradle we need to add the pebble dependencies and repositories.

        dependencies {
    		compile 'com.getpebble:pebblekit:2.6.0'
    	}
    	repositories {
    		mavenCentral()
    		maven { url "https://oss.sonatype.org/content/groups/public/"}
    	}

It is very important that you resync your project at this point. It may take a
while as it downloads pebblekit.

Pebble uses JSON structures to communicate packets of data between the watch
and companion app so the following imports must be included.

	import org.json.JSONArray;
	import org.json.JSONObject;

Finally we need to build our notification to send. This should be included in
`didFindCampaign(Campaign c)` just like the Android Wear notification.

	final Intent i = new Intent("com.getpebble.action.SEND_NOTIFICATION");
        final Map data = new HashMap();
        data.put("title", c.getTitle());
        data.put("body", c.getMessage());
        final JSONObject jsonData = new JSONObject(data);
        final String notificationData = new JSONArray().put(jsonData).toString();
        i.putExtra("messageType", "PEBBLE_ALERT");
        i.putExtra("sender", "MyApplication");
        i.putExtra("notificationData", notificationData);
        sendBroadcast(i);

This type of notification can only be dismissed. ~~However, the recent Pebble Beta
claims to support Android Wear type notifications. As the beta continues 
surely we will see more information on how to generate that kind of notification 
on Pebble.~~ The recent Pebble Beta supports Android Wear style notifications
following the same procedure as for the Android Wear device. Keep in mind
when testing that Pebble does not display duplicate messages. So everytime you
send a test notification to the Pebble make sure to change the **ContentText** in
order for the notification to actually appear.
