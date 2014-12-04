---
layout: post
title: Fundamentals of Beacon Ranging
author: David G. Young
---

The most basic use of beacon technology is to determine how far a mobile device is from a beacon, but as anybody who has played with beacon ranging knows, these distance estimates can have a significant degree of uncertainty.  For a beacon that is 5 meters away, distance estimates might fluctuate between 2 meters and 10 meters.  

The reasons for these distance estimate variations and the steps that can be taken to reduce them are some of the most frequent questions we get about beacons.  Factors that influence the error in the estimate include reflections of the radio signal, obstructions that attenuate the radio signal, and orientation of both the phone and the beacon.  

By far the biggest factor affecting error in distance estimates is radio noise.  Background radio noise exists everywhere (it's what causes the static seen on an old analog TV).  For strong radio signals (a high signal-to-noise ratio), noise is far less of an issue than with weak signals (a low signal to noise ratio).  For this reason, a device that is within a few meters of a beacon can provide much more accurate distance estimates than a device that is 10 meters or more away.

##Apple's Recommendations

When Apple introduced iBeacon technology in iOS 7, their documentation recommended against direct use of distance estimates.  The `CLBeacon` class that provides access to beacon ranging information has a field that provides an estimate of the distance to the beacon in meters.   But instead of calling this property `distance`, Apple called it `accuracy`, probably in an effort to discourage its use as a raw distance indicator.  The recommended approach is to use it only for comparison between beacons to see which is closest.  The `CLBeacon` class also provides a second property called `proximity` which bucketizes the distance estimate into “immediate”, “near” and “far” groupings.  Apple is vague at the definitions of these buckets, but experimentation shows that “immediate” means about 0.5 meters or less, “near” means about three meters or less, and “far” means more than three meters.

This doesn't mean that you can't use beacon distance estimates directly — it just means you first need to understand the fundamentals of how they work and the limitations on the quality of information they can provide.

##Reference Transmitter Power

A beacon transmission includes a transmitter power field that indicates how strong the signal should be at a known distance.  As an example, when using iBeacon technology, the standard measurement is the signal level that an iPhone 5 (the latest model available when iOS 7 was released) will measure for a beacon at a distance of one meter.  Beacons must be calibrated  by measuring the signal level (known as Received Signal Strength Indicator or RSSI) at this reference distance and then configuring the beacon to transmit reference value.  Values of RSSI are measured in dBm, and a typical one meter calibration value for a beacon transmitter will be -59 dBm.  The actual calibration value for different beacons varies because each may have a transmitter and antenna that puts out a different amount of radio power.  Some beacons, such as Radius Networks' [RadBeacon products](http://store.radiusnetworks.com/), have configurable transmitter output power.

##How Distance Estimates Work

Mobile devices can estimate the distance to the beacon by comparing the signal level they see with the reference signal level.  Each time beacon advertisement packet is received, the bluetooth chipset provides a measurement of the beacon's signal level as RSSI.  Because every single beacon transmission also includes the calibration value mentioned above, it is possible to compare the actual signal level with the expected signal level at one meter and then estimate the distance.  For example, let's say the beacon advertisement packet was received with a signal level of -65 dBm and the transmitter power calibration value sent inside the transmission was -59 dBm.  Because -65 dBm represents a weaker signal level than -59 dBm (greater negative numbers represent weaker signals) this means the beacon is probably more than one meter away.  

You can plug these two numbers into a formula to calculate a distance estimate.  Below, you can see the formula we created in the [Android Beacon Library](https://altbeacon.github.io/android-beacon-library/configure.html).  The three constants in the formula (0.89976, 7.7095 and 0.111) are based on a best fit curve based on a number of measured signal strengths at various known distances from a Nexus 4.

```java
protected static double calculateDistance(int txPower, double rssi) {
  if (rssi == 0) {
    return -1.0; // if we cannot determine distance, return -1.
  }

  double ratio = rssi*1.0/txPower;
  if (ratio < 1.0) {
    return Math.pow(ratio,10);
  }
  else {
    double accuracy =  (0.89976)*Math.pow(ratio,7.7095) + 0.111;    
    return accuracy;
  }
} 
```

With our examples above, if you plug in -65 for txPower and -59 for rssi, the formula above returns a distance estimate of 2.0 meters.

##Filtering for noise

Because of the signal noise, it is usually not a good idea to do a distance estimate based on a single RSSI measurement.  If you look at the raw RSSI of beacon packets, the signal level jumps all over the place.  The simplest way to filter out this noise is to use a running average of RSSI measurements — e.g. the most recent 20 seconds worth — to help smooth out the noise.  It is also common to throw out particularly high or particularly low numbers in these data sets.  The algorithm we use in the Android Beacon Library discards the top and bottom 10% of measurements over the 20 second sample period and averages the rest.  

The advantage of this approach is that the distance estimates are much more accurate and stable.  The disadvantage is that as a mobile device moves relative to the beacon, it will take 20 seconds for the distance estimate to catch up with where the device moved relative to the beacon.  

A 20 second sampling period is similar to what is done by iOS.  The graph below shows how the `CLBeacon` accuracy field changes over time when an iPhone 4S is quickly moved from a distance of 0.5 meters to 3 meters away (red line).  As you can see, it takes about 20 seconds for the distance estimate (blue line) to stabilize after the device is moved. 

<img style="margin:10px; border: thin solid #333;" src='../img/ranging-averaging.png'>

##Device Variations

Beacon distance estimates behave consistently across most iOS devices because there is not much variation in the bluetooth radios, antennas, and case designs between the iPhone 4S, iPhone 5 and iPhone 6 models, but this is not universally true.  Distance estimates from iPod Touch devices will generally underestimate the distance to beacons, most likely because their antennas are placed differently to provide a bit more gain in receiving the Bluetooth radio signals.  

Device variation is a far greater problem for Android.  Each Android model may have a completely different bluetooth chipset, antenna, and case design.  All of these factors affect the received signal level and therefore the distance estimates.  Nexus 4, Nexus 5 and Samsung Galaxy S4 models receive significantly different signal levels from the same beacon at the same distance.  For this reason, Radius Networks has started building a database of distance formula coefficients for various Android devices.

##Setting Expectations

While it is possible to estimate the distance to a beacon, it must be stressed that these are just estimates.  As devices get further away from the beacon, the estimates get progressively less accurate.  A device that is 20 meters away from a beacon might get a distance estimate with an error of +/- 10 meters.  Depending on whether a phone is in a pocket, oriented in a different direction, behind a sales display, or blocked by a crowd of people, the estimate of the distance to a beacon may be affected.  

It is therefore important to realize what you can't do:

* While you can easily tell if a beacon is one meter away or 10 meters away, don't expect to be able to tell for sure if one is 10 meters away or 20 meters away.

* Because running averages are used to filter out noise, don't expect the distance estimates to change in near real-time as a user moves around.

* Don't expect to be able to determine the direction to a beacon.  Because beacons are typically omnidirectional transmitters, although you can estimate distance, you cannot estimate direction.

* Realize that being able to estimate the distance to a beacon does not tell you exactly where a mobile device is in a room.  While beacons can be used as a building block within an indoor location system, beacon distance estimates do not by themselves solve this problem.  

* Don't expect to be able to use trilateration with a few beacons to create a simple indoor location system.  Because accuracy of distance estimates gets worse at greater distances, using trilateration to determine position within a room cannot provide accurate estimates of position.

That said, there are still useful things you can do with beacon distance estimates.  Here are some examples:

* Trigger an action to happen when a beacon is very close by, ie., 5 meters or less.

* Determine which of several visible beacon is significantly closer, provided that the closest beacon is within a few meters.

* Provide distance feedback to a user who is looking for a particular location, e.g. in a scavenger hunt.

##Best Practices

In order to get the best possible distance estimates, Radius Networks recommends the following:

* Use the strongest transmitter power level supported by your beacon if possible and if the use case allows for it.  Stronger signal levels mean higher signal to noise ratios and better distance estimates.

* Use the highest transmission frequency supported by your beacon.  The more advertising packets received by a mobile device, the more samples it will have to average to filter out noise.

* Use powered beacons where possible, like Radius Networks' [RadBeacon USB](http://www.radiusnetworks.com/ibeacon/radbeacon/), so that you can transmit at high power and high frequency without worrying about battery life.

* Calibrate your beacons properly.

* Place beacons in locations with a clear line of sight to where the signals will be received.  If in a crowded location, placements overhead work better than those at ground level.

* If you are planning on deploying an app for a specific Android model, make sure that you have a distance estimation formula optimized for that model.  Radius Networks' [Android Beacon Library](https://altbeacon.github.io/android-beacon-library) supports an expandable database of distance calculations for various Android models.
