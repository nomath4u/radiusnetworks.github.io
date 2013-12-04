---
layout: ibeacon
---

<div align="center">
<img src="/img/scanbeacon_icon.png" style="width: 100px; margin-top: 10px;">
<h2>Download ScanBeacon</h2>
<p>Thank you for your purchase!<p>
<p>Please <a href="https://s3.amazonaws.com/downloads.radiusnetworks.com/c5409e56-52f3-43cd-a77b-18afaa2ec5ec/ScanBeacon.app.zip">click here</a> to begin your download.</p>
</div>
<hr>

##How-to Install ScanBeacon

1. By clicking the link above, your download of the ScanBeacon.app.zip file should begin automatically. Once the download is complete, browse to your download's destination folder and unzip the package.

2. Drag the ScanBeacon icon <img src="/img/scanbeacon_icon.png" style="width: 35px; padding-bottom: 5px;"> to your applications folder.

3. When you are ready to launch ScanBeacon, double click the icon.

##ScanBacon Help

ScanBeacon is the iBeacon scanner for your Mac. With the ScanBeacon app from Radius Networks, you can discover and monitor nearby iBeacon proximity beacons using any Macintosh computer running OS X 10.9 and equipped with Bluetooth 4.0 support.

####What is an iBeacon?

iBeacons are low-power wireless transmitters that send identifying information to nearby mobile devices using Bluetooth Low Energy (also known as Bluetooth 4.0 and Bluetooth Smart) signaling. Applications on mobile devices can be triggered by these proximity events and can implement behaviors that correspond to entering, traversing or leaving the local region covered by the iBeacon.

The benefit of iBeacons is that they provide hyper-location awareness to mobile devices. iBeacon regions range up to approximately 100 feet, and mobile devices can recognize their proximity to an iBeacon with an accuracy of about 1 foot. Mobile apps can be designed to recognize specific iBeacons and present special promotions, personalized messages or location-specific services that are relevant, timely and valuable to the mobile device user.

For example, imagine you walk into a major retailer with the retailer’s mobile app installed on your iPhone or other device. The mobile app will be notified as you approach an iBeacon installed in the shoe department. The mobile app recognizes the iBeacon’s region identifier and fetches an offer for the latest inventory of designer shoes that is relevant, timely and valuable to you based on your location and the retailer’s understanding of your purchasing preferences.

<img style="width: 70%; margin: 5px 0 5px 70px;" src="/img/store.png">

##Using ScanBeacon

With the ScanBeacon app you can scan for nearby iBeacon proximity beacons and display their advertising details using any Macintosh computer running OS X® 10.9 or higher and equipped with built-in or third party Bluetooth 4.0 support.

In the ScanBeacon application window, start scanning for beacons by clicking the ‘Start Scanning’ button. ScanBeacon will display a list of the detected beacons and their advertising details, as shown in the figure below. ScanBeacon will continuously update the list at 1 second intervals while it is actively scanning. iBeacons that have not transmitted an advertisement for at least 10 seconds are removed from the displayed list.

<img style="width: 70%; margin: 5px 0 5px 70px;" src="/img/macbeacon-window.png">

Clicking on the ‘Stop Scanning’ button will terminate the scanning process and leave a static display of the detected iBeacons that were in the list at the time that scanning was halted.

####iBeacon Advertising Details

The beacon list displays the detection status and the iBeacon details of detected beacons. The detail items for each listed beacon are defined as:

Active – This indicator displays the advertising status of the the listed beacon. This value is derived based on the elapsed time since the last received iBeacon advertisement. The indicator is GREEN when an advertisement has been received within the last 5 seconds. The indicator is GREY when more than 5 seconds has elapsed since the last received advertisement. If more than 10 seconds has elapsed since the last received iBeacon advertisement, the entry in the list is removed.

<b>UUID</b> – The UUID identifier transmitted by the beacon. UUID identifiers use hexadecimal values (0-9 or A-F) in the following format: 01234567-89AB-CDEF-0123-4567890ABCDE. UUID identifiers are expected to be unique to the organization that deployed the beacon.

<b>Major</b> – The group identifier that will be transmitted by the beacon. Group identifiers must be a decimal value between 0 and 65535.

<b>Minor</b> – The individual identifier that will be transmitted by the beacon. Individual identifiers must be a decimal value between 0 and 65535.

<b>Power</b> – The measured power value transmitted by the beacon. This value is set during beacon deployment and calibration. The value is intended to reflect the received signal strength indicator (RSSI) value (measured in decibels) for the device, and represents the measured strength of the beacon from one meter away during ranging.

<b>RSSI</b> – Relative Signal Strength Indication, this value is a measure of the power level of the signal being received by the scanner.

<b>Distance</b> – The distance of the scanning receiver to the beacon. This value is derived from the Power and RSSI values and is largely dependent on the accuracy of these two values.

<b>Proximity</b> – The proximity of the scanning receiver to the beacon. This is a qualitative value derived from the Distance calculation. In general, Proximity is evaluated as

* Immediate – 0.0m to 0.5m
* Near – 0.5m to 4.0m
* Far – more than 4.0m

##Turning Beacons On and Off

You can turn a beacon on and off by either double clicking on a beacon in the Beacon List or by selecting the beacon from the list and clicking the Turn Beacon On key to enable the beacon or the Turn Beacon Off key to disable the beacon. Only one beacon can be enabled at a time.

##Sharing iBeacon Scanning Results

Being able to copy and paste iBeacon advertisement information is very useful for quickly configuring other applications and for sharing scan result information with colleagues.

iBeacon information can be copied and pasted to other applications by selecting an iBeacon in the list and pressing the CMD-C key combination, or selecting ‘Copy Advertisement’ under the Beacon menu item or from the CTRL-click accessible context menu in the application window.

Information copied to the clipboard as a textual representation and can be pasted into any application that will accept plain text. An example of the copied information is shown below.

        {
            identifier = "E67DD696-409B-4B03-8156-1079926D7D31";
            major = 0;
            minor = 1;
            power = "-59";
            rssi = "-78";
            timestamp = "1386087720.724988";
            uuid = "74278BDA-B644-4520-8F0C-720EAF059935";
        }
        
The complete results of a scan operation can be saved to a file which can be reloaded into any instance of the ScanBeacon application. Scan results are saved in JSON-encoded files with the .ibeacon extension.

To save the results of a scan operation, terminate any active scanning session and select ‘Save’ or ‘Save As...’ under the ‘File’ menu item. The .ibeacon file extension is added automatically to any filename that you specify.

To load previously saved results, terminate any active scanning session and select ‘Open...’ under the ‘File’ menu item and select the file to load. The advertising information from the loaded file will be displayed in the beacon list along with the derived distance and proximity values calculated based on the RSSI value at the time the information was saved.

##Applications & Tools to Use for Testing

It’s early days in the market for iBeacon-based tools and technology. Radius Networks provides several ways for you to create iBeacons, including

<b>iBeacon Development Kit</b> – A development platform for creating stand-alone iBeacons based on the Raspberry-Pi embedded computing paltform. <a href=http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit.html >More information is available here.</a>

<b>MacBeacon</b> – An OS-X application that enables any Macintosh computer running OS X® 10.9 or higher and equipped with built-in or third party Bluetooth 4.0 support to implement a library of virtual iBeacons. <a href=http://radiusnetworks.com/macbeacon-app.html >More information is available here.</a>

<b>Locate for iBeacon</b> – An iOS application that enables any iPhone, iPad or iPod Touch running iOS 7 and equipped with Bluetooth 4.0 to implement a library of virtual iBeacons and scan for iBeacons. <a href=https://itunes.apple.com/us/app/ibeacon-locate/id738709014?ls=1&mt=8 >More information is available here.</a>

<b>iBeacon Locate</b> – An Android application that enables any Android device running Android 4.3 and equipped with Bluetooth 4.0 to implement a library of virtual iBeacons and scan for iBeacons. <a href=https://play.google.com/store/apps/details?id=com.radiusnetworks.ibeaconlocate&hl=en >More information is available here.</a>

At Radius Networks we have a whole portfolio of tools and services for iBeacon app developers. Visit the Radius Networks <a href=http://www.radiusnetworks.com/ibeacon.html >iBeacon Services</a> page to get the latest updates.

##For Application and Solution Developers

For Application and Solution developers, Radius Networks provides the Proximity Kit Software Development Kit to rapidly accelerate proximity-based mobile applications for iOS and other mobile devices.

Proximity Kit provides everything you need to enable your mobile applications with geofence and iBeacon proximity-awareness and to tie those events to real-world experiences and services for your clients and customers.

Backed by Radius Networks proximity cloud services, Proximity Kit provides you everything you need to start delivering iBeacon proximity solutions today. Visit Proximity Kit on the web to learn more

##Getting Help

If you have problems configuring and operating the MacBeacon software. Support is available at <a href="http://www.radiusnetworks.com/suport.html">Radius Networks Customer Support.</a>



