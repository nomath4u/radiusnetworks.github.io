---
layout: ibeacon-m
---
##iOS iBeacon Locate Help

###Filtering iBeacons

By default, iBeacon Locate will show all visible iBeacons that have any of the Proximity UUIDs configured in the app.
The default list includes several popular development Proximity UUIDs from Apple's AirLocate example app as well as from Radius Networks and other
iBeacon suppliers.  If you can't see an iBeacon, you may need to add its UUID to the app.  See the section on Configuring iBeacon Identifiers for instructions on how to do so.

###Alerting for iBeacons

iBeacon Locate can alert you whenever one or more iBeacons with the identifiers in your configuration list are nearby by displaying an alert.

This feature can be configured by going to settings and enabling the Notify on Entry, Notify on Exit options.

Because iOS limits the number of iBeacon regions you can monitor, if you have more than 20 iBeacon regions defined in your configuration list, only the first 20 will be monitored.

###Distance Measurements

The iBeacon Locate app will show an estimate of the distance to any visible iBeacon.  This estimate is based on the Bluetooth signal strength, and due to noise and multipath effects there is a large amount of variation.  

The distance estimate is based on a transmitter power calibration value stored in the iBeacon and transmitted with each of its advertisements.  In order to optimize the accuracy of the distance estimate, it is necessary to calibrate each iBeacon's transmitter power and then store the configuration measurement in the iBeacon.

###Calibrating iBeacons

You can calibrate an iBeacon by tapping it in the app, selecting the calibration option, holding your phone or tablet one meter away from the iBeacon's transmitter, then tapping the calibrate button.  Calibration will take from 30 seconds to one minue, during which time you should keep your device at a distance of one meter from the iBeacon.    

### Transmitting as an iBeacon

Your phone can act as an iBeacon by selecting this option.  After tapping the button, you see a list of the pre-defined iBeacons that you can use to broadcast.  (You may also define your own -- see the Configuring iBeacon Identifiers section.)
Tap one of the rows in the list and you will begin transmitting automatically at a rate of once per second.

### Configuring iBeacon Identifiers

iBeacon Locate includes an internal list of of known iBeacons and iBeacon Regions that it knows about.  At installation, this list includes a list of 
iBeacon identifiers for development models from popular vendors.  You may also add your own.  The same configuration list is used for all three purposes, but each configured iBeacon is used in slightly different ways for each.

1. Giving you alerts on when a beacon is nearby, an iOS feature known as "monitoring."

    When monitoring, the Power field is unused.  The Name field is shown in any alerts displayed on the phone.  The Major and Minor fields may be left blank, and if they are, any iBeacon with the supplied Proximity UUID will be matched for monitoring purposes.


2. Locating which iBeacons are nearby, an iOS feature known as "ranging."

    When ranging, only the Proximity UUIDs from the list are used.  iBeacon Locate will show you any iBeacon matching any of the UUIDS in the configuration list.

3. Transmitting as an iBeacon.
    
    When transmitting, all fields are used except the Name field.  The Power field is a configuration value of the bluetooth transmitter power of your iOS device.  If left blank, it defaults to -59.  The Major and Minor if left blank, default to 0.




 
