---
layout: post
title: iOS 7.1 Background Beacon Detection Times
author: David G. Young
---

The release of iOS 7.1 has led to some hope that the new release will improve beacon background detection times.  Repeating [earlier tests](/2013/11/13/ibeacon-monitoring-in-the-background-and-foreground.html) on iOS 7.0 shows this not to be the case — it can still take up to 15 minutes to detect an beacon in the background.

These tests used a iPhone 4S recently upgraded to iOS 7.1 running an [Beacon BackgroundDemo program available on GitHub](https://github.com/RadiusNetworks/ibeacon-background-demo).  The same program had been used to test background detection times on iOS 7.0, and the full test setup and procedure is described in an [accompanying blog post](/2013/11/13/ibeacon-monitoring-in-the-background-and-foreground.html).   For an beacon transmitter, the test used a [Radius Networks Dual Beacon Development Kit](http://store.radiusnetworks.com/collections/all) (with two transmitters), that send out advertisements at a frequency of 10Hz when enabled.   Cycling these transmitters on and off produced the following annotated log file (annotations start with #):


        2014-03-11 16:53:41.499 BackgroundDemo[287:60b] applicationDidFinishLaunching
        2014-03-11 16:53:41.522 BackgroundDemo[287:60b] applicationDidBecomeActive
        2014-03-11 16:53:48.464 BackgroundDemo[287:60b] applicationWillResignActive
        2014-03-11 16:53:48.493 BackgroundDemo[287:60b] applicationDidEnterBackground
        #          16:54:30 started region 1 transmitting @ 10 Hz
        2014-03-11 16:55:26.833 BackgroundDemo[287:60b] locationManager didDetermineState INSIDE for region1
        #          16:57:11 stopped region 1 transmitting, started region 2 @ 10Hz
        2014-03-11 17:10:31.305 BackgroundDemo[287:60b] locationManager didDetermineState INSIDE for region2
        2014-03-11 17:10:34.276 BackgroundDemo[287:60b] locationManager didDetermineState OUTSIDE for region1
        #          17:14:48 stopped all iBeacons from transmitting
        2014-03-11 17:40:34.325 BackgroundDemo[287:60b] locationManager didDetermineState OUTSIDE for region2
        #          17:50:52 started region 1 transmitting @ 10 Hz
        2014-03-11 17:55:31.336 BackgroundDemo[287:60b] locationManager didDetermineState INSIDE for region1
        #          17:56:36 stopped region 1 transmitting
        2014-03-11 18:10:31.339 BackgroundDemo[287:60b] locationManager didDetermineState INSIDE for region2
        2014-03-11 18:10:34.308 BackgroundDemo[287:60b] locationManager didDetermineState OUTSIDE for region1
        #          18:19:57 stopped all iBeacons from transmitting
        2014-03-11 18:25:34.353 BackgroundDemo[287:60b] locationManager didDetermineState OUTSIDE for region2


Normalizing the timestamps to start 15 minutes before the first beacon detection, and plotting both detections (blue) and the beginning/ending of transmissions (red) vs time, you see an interesting pattern:

<img src="/img/ios7_1_detection.png"/>￼￼

The first thing you notice is that all the entry/exit detections (blue) are exactly on 15 minute time boundaries.  (Looking closer at the data, you will see that this varies by a few seconds, but it is still very, very regular.)  This suggests that iOS is only looking for beacons once every 15 minutes.  

Zooming in on the chart at the 0:30:00 mark, you can see that the exit detection (light blue) comes three seconds after the region entry detection.  

<img src="/img/ios7_1_detection_zoom.png"/>￼

So if a bluetooth scan starts once every 15 minutes, and lasts 3 seconds, an detection might take place exactly on the 15 minute boundary, and a failure to detect (which would lead to a region exit event) would take place three seconds after the 15 minute boundary.

There is one anomaly on the first chart.  Note that there is no blue dot indicating an entry/exit detection at 0:45:00, even though an beacon had stopped transmitting about 12 minutes earlier, which should have caused an exit condition.  What happened here?  Did iOS fail to do a bluetooth scan on schedule?  Whatever happened, the region exit did not fire until the next 15 minute cycle, over 26 minutes after the beacon was turned off.  This delay is significantly larger than anything seen during similar tests on iOS 7.0.

The 26 minute anomaly aside, these results are consistent with my last tests done with iOS 7.0.  We can conclude is that iOS 7.1 does not improve the speed of detecting beacons in the background — at least for the conditions under test on an iPhone 4S.  
