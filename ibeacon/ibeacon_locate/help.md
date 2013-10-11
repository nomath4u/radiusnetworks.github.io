---
---
##iBeacon Locate Help

###Filtering iBeacons

By default, iBeacon Locate will show all visible iBeacons.  You may filter the iBeacons the app sees by going to settings and editing the "iBeacon Region Filter"  These settings allow you to optionally select a proximityUUID, a major, and a minor identifier for the iBeacons you want to see.  Only iBeacons matching the selected field values will be visible by the app.

###Alerting for iBeacons

iBeacon Locate can alert you whenever one or more iBeacons are nearby by displaying an alert.

This feature can be configured by going to settings, selecting "Monitoring Configuration" and enabling the Notify on Entry, Notify on Exit options.

###Distance Measurements

The iBeacon Locate app will show an estimate of the distance to any visible iBeacon.  This estimate is based on the Bluetooth signal strength, and due to noise and multipath effects there is a large amount of variation.  

The distance estimate is based on a transmitter power calibration value stored in the iBeacon and transmitted with each of its advertisements.  In order to optimize the accuracy of the distance estimate, it is necessary to calibrate each iBeacon's transmitter power and then store the configuration measurement in the iBeacon.

###Calibrating iBeacons

You can calibrate an iBeacon by tapping it in the app, selecting the calibration option, holding your phone or tablet one meter away from the iBeacon's transmitter, then tapping the calibrate button.  Calibration will take from 30 seconds to one minue, during which time you should keep your device at a distance of one meter from the iBeacon.    


 
