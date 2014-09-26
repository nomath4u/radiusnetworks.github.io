---
layout: post
title: Announcing Proximity Kit Android SDK
author: Aaron Kromer
---

The Radius Networks team is committed to building the features and services
necessary to promote Android as a first-class citizen in the world of beacons
and proximity services, so we are thrilled to announce the re-release of our
[Proximity Kit Android SDK](https://github.com/RadiusNetworks/proximitykit-android).

This is a major update which not only provides support for geofences through
Google Play services, but also leverages the open
[AltBeacon](http://altbeacon.org) standard for beacon detection. While we were
at it, we made a few other changes to improve the Proximity Kit experience on
Android.

The Radius Networks Proximity Kit Android SDK now uses JDK 7. This change
allows us to leverage more features of the Java language internally, enabling
us to provide a better SDK to you.

If you used our prior version of this SDK you may have felt some pain in how
you were required to implement the callback notifiers. With this new release,
we are pleased to announce we have improved this process by partitioning the
callback notifiers based on their shared behavior. You can now implement just
the notification subset(s) you care about.

Feel free to peruse these changes in our updated open source [sample
application](https://github.com/RadiusNetworks/proximitykit-reference-android).
Or just head on over to [Proximity Kit](https://proximitykit.radiusnetworks.com/)
to get started on your app today!
