---
layout: post
title: Beacons Don't Track People, Apps Track People
author: Marc Wallace
---

We see a lot comments and posts in the media that mistakenly tout proximity beacons as a way to 'track' customers.  This has become yet another common misconception of beacons, so I'd like to clarify how beacons relate to the issue of privacy.

Beacons are transmit only. They do not receive or collect any signals from mobile devices. Beacons don't detect the presence of your mobile device and therefore have no ability to know you are near or track your location.  The bottom line, iBeacon™ technology is inherently privacy friendly.  You can see them, but they can't see you.

With iBeacon™ technology, your mobile device is actually what detects the iBeacons. More specifically, an app installed on the mobile device can ask to be notified when the device sees a specific beacon. This works very similar to how geofences work when a mobile device crosses into a specific geographic location.

Keep in mind that in order for a mobile device to detect and react to an beacon, an app MUST reside on the device and have requested the specific beacon identifiers it is interested in. The benefit of this approach is that it gives the user ultimate control.  If a user does not want to interact with beacons, he or she can opt-out by not allowing the app to use location services (iOS), turning Bluetooth off, or uninstalling the app on their phone.

Once an app is notified by the mobile device that it has seen a beacon, the app can respond by taking some action based on the logic implemented by the app provider. One of those actions may be to report the beacon information to a back-end server. The owner of the app may want data on when and where mobile devices see beacons.  In terms of privacy, the transmission of this beacon data is really no different than transmitting GPS location data, which is something that has been going on as long as smartphones have been around.

The application provider may want information that confirms that their beacons are working correctly, or may want analytical information on how often specific users encounter specific beacons so that they can provider tailored and personalized services.  These are two very different scenarios with different privacy implications.  However, it is important to note that it is the mobile apps, not the beacons, that are reporting customer proximity information back to the app provider.

As a member of the Future Privacy Forum's industry working group, Radius Networks is actively working to establish a responsible privacy framework and best-practice guidelines for apps reporting proximity information.  For more details, please see the [Mobile Location Analytics Code of Conduct](http://smartstoreprivacy.org/mobile-location-analytics-opt-out/about-mobile-location-analytics-technology/).

The fact that a user can 'opt-out' of beacon proximity interaction, as described previously, gives the user the power to manage and control whether proximity information is available to the app providers.  In turn, companies providing apps should implement mechanisms that recognize the user's intent and be as transparent as possible with their customers to make sure that these options are properly communicated.

Other guidelines include anonymizing any identifiers used by apps to feed analytics data and limiting the retention of any device-specific data records for only as long as needed to convert to aggregate analyses or as allowed by an explicit agreement with the user.

Beacons are powerful enablers for delivering new personalized services to customers. App providers leveraging beacon technology and the resulting proximity information have a responsibility to ensure that users are fully informed of these activities and aware of the opt-out options available to them.  At Radius Networks, we feel that implementing a strong privacy framework and following best practices that protect users' privacy will be required for providers and users alike to fully benefit from the potential of this exciting new technology.

