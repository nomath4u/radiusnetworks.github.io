---
layout: ibeacon
---

<div align="center">
<img src="/img/macbeacon_icon.png" style="width: 100px; margin-top: 10px;">
<h2>Download MacBeacon</h2>
<p>Thank you for your purchase!<p>
<p>Please <a href="https://s3.amazonaws.com/downloads.radiusnetworks.com/48d0883a-69c5-48f5-9e74-ee0d2cdf82e9/MacBeacon.app.zip">click here</a> to begin your download.</p>
</div>
<hr>

##How-to Install MacBeacon

1. By clicking the link above, your download of the MacBeacon.app .zip file should begin automatically. Once the download is complete, browse to your download's destination folder and unzip the package.

2. Drag the Macbeacon icon <img src="/img/macbeacon_icon.png" style="width: 35px; padding-bottom: 5px;"> to your applications folder.

3. When you are ready to launch Macbeacon, double click the icon.

##Macbeacon Help

With the MacBeacon app you can turn any Macintosh computer running OS X® 10.9 or higher and equipped with built-in or third party Bluetooth 4.0 support into your own, fully-configurable, iBeacon proximity beacon.

####What is an iBeacon?

iBeacons are low-power wireless transmitters that send identifying information to nearby mobile devices using Bluetooth Low Energy (also known as Bluetooth 4.0 and Bluetooth Smart) signaling. Applications on mobile devices can be triggered by these proximity events and can implement behaviors that correspond to entering, traversing or leaving the local region covered by the iBeacon.

The benefit of iBeacons is that they provide hyper-location awareness to mobile devices. iBeacon regions range up to approximately 100 feet, and mobile devices can recognize their proximity to an iBeacon with an accuracy of about 1 foot. Mobile apps can be designed to recognize specific iBeacons and present special promotions, personalized messages or location-specific services that are relevant, timely and valuable to the mobile device user.

For example, imagine you walk into a major retailer with the retailer’s mobile app installed on your iPhone or other device. The mobile app will be notified as you approach an iBeacon installed in the shoe department. The mobile app recognizes the iBeacon’s region identifier and fetches an offer for the latest inventory of designer shoes that is relevant, timely and valuable to you based on your location and the retailer’s understanding of your purchasing preferences.

<img style="width: 70%; margin: 5px 0 5px 70px;" src="/img/store.png">

##Configuring MacBeacon

iBeacons are generally available as stand-alone devices using specialized hardware. With the MacBeacon app you can turn any Mac running OS X 10.9 or higher and equipped with built-in or third party Bluetooth 4.0 support into your own, fully-configurable, iBeacon proximity beacon.

MacBeacon allows you to create a list of beacon profiles that you can easily adjust and switch between. In addition to providing the full functionaliy of an iBeacon for your own use, the ability to create and maintain multiple profiles makes MacBeacon a very effective tool for developing and testing iBeacon-based proximity solutions for your organization and your customers.

<img style="width: 70%; margin: 5px 0 5px 70px;" src="/img/macbeacon-window.png">

MacBeacon is divided into two views, with the Beacon List shown on the left and the Beacon Detail shown on the right.

####Beacon List

The Beacon List view is a table of the beacon profiles that you have currently defined. You can add and remove beacon profiles using the + and - buttons. Modifications and changes to the beacon profiles are done through the Beacon Detail view.

####Beacon Detail

The Beacon Detail view displays the operational status and profile details of each beacon when you select that beacon from the Beacon List view. The Beacon Detail view also provides you a button for turning the beacon on and off.

####Beacon Profiles

Each beacon profile contains the operational parameters for that beacon. You can add, remove and change beacon profiles, but you must always have at least 1 profile defined. The operational parameters contained in a beacon profile are:

<b>Major</b> – The group identifier that will be transmitted by the beacon. Group identifiers must be a decimal value between 0 and 65535.

<b>Minor</b> – The individual identifier that will be transmitted by the beacon. Individual identifiers must be a decimal value between 0 and 65535.

<b>Power</b> – The power value for the beacon. This value is adjustable to allow calibration of the beacon. The value reflects the received signal strength indicator (RSSI) value (measured in decibels) for the device, and represents the measured strength of the beacon from one meter away during ranging.

MacBeacon ships with 4 default beacon profiles. Three of these profiles come from the Apple AirLocate sample app which is available to registered Apple iOS developers as an introduction into developing iBeacon-based mobile apps for iOS devices. The fourth is the Radius Networks Demo beacon profile which is used in sample applications made available by Radius Networks.

##Turning Beacons On and Off

You can turn a beacon on and off by either double clicking on a beacon in the Beacon List or by selecting the beacon from the list and clicking the Turn Beacon On key to enable the beacon or the Turn Beacon Off key to disable the beacon. Only one beacon can be enabled at a time.

##Beacon Status

Operational status for each beacon is shown by the status indicator and at the left of each beacon name and the status label just below each beacon name in the Beacon List. Each beacon can be in one of three major states.

<ul style="list-style-type: none;">
  <li><b>Beacon On</b>: Represented by a <span style="color: #83D821;">GREEN</span> status indicator</li>
  <li><b>Beacon Off</b>: Represented by a <span style="color: #999999;">GREY</span> status indicator</li>
  <li><b>Beacon Error</b>: Represented by a <span style="color: #D83A21;">RED</span> status indicator</li>
</ul>

The Beacon Error state indicates a problem with the Mac’s Bluetooth services. In the event of a Beacon Error, please check that your Mac supports Bluetooth 4.0 (Bluetooth Low Energy) and that Bluetooth is turned on in the System Preferences.

##Applications to Use for Testing

It’s early days in the market for iBeacon-based applications. Apple’s AirLocate sample app is a good starting point, and is available as one of several <a href="https://developer.apple.com/downloads/index.action?name=WWDC%202013">WWDC 2013</a> samples for registered iOS developers. Radius Networks’ iBeacon Locate app is currently available. You can find a link for this and other iBeacon related information on the <a href="http://www.radiusnetworks.com/ibeacon.html">Radius Networks iBeacon Services page</a>.

##For Application and Solution Developers

For Application and Solution developers, Radius Networks provides the Proximity Kit Software Development Kit to rapidly accelerate proximity-based mobile applications for iOS and other mobile devices.

Proximity Kit provides everything you need to enable your mobile applications with geofence and iBeacon proximity-awareness and to tie those events to real-world experiences and services for your clients and customers.

Backed by Radius Networks proximity cloud services, Proximity Kit provides you everything you need to start delivering iBeacon proximity solutions today. Visit Proximity Kit on the web to learn more

##Getting Help

If you have problems configuring and operating the MacBeacon software. Support is available at <a href="http://www.radiusnetworks.com/suport.html">Radius Networks Customer Support.</a>

a




