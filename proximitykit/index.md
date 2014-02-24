---
layout: proximitykit
---

<img style="width:800px;height:178px;" src="/img/proximitykit-logo.png" />

Designed to help developers build location aware apps. By providing a rich SDK built on top of the latest Geofence and iBeacon technology, Proximity Kit gives you the events you need to keep your app relevent and useful to your users.

---

## The Lifecycle of a Proximity Kit

<div class="tiles clearfix">
  <div class="tile">
    <img class="tile-image" src="/img/pk-configure.png" />
    <h3>1. Configure Geofences and iBeacons in the Web Portal</h3>
    <p>Configure and manage your Geofences and iBeacons in our simple web portal. This is where you setup your kit, normally one kit per app, and set the region attributes.</p>
  </div>
  <div class="tile">
    <img class="tile-image" src="/img/pk-cloud.png" />
    <h3> 2. Mobile SDK Syncs with Mobile Device </h3>
    <p> When the app starts up the Proximity Kit Manager, it will sync with our backend. As that happens the SDK will register each reagion to monitor. Your region data and configuration is cached and updated in the background.</p>
  </div>
  <div class="tile">
    <img class="tile-image" src="/img/pk-monitor.png" />
    <h3> 3. Device Monitors for Proximity Events </h3>
    <p> When your app is in the background, we'll monitor for proximity events, even after the user restarts their phone. When your app is in the foreground, Proximity Kit will provide detailed information about the iBeacons or GPS coordinates around it.</p>
  </div>
</div>

Upon entering or leaving an iBeacon or Geofence region, Proximity Kit notifies your app of the proximity event along with region identifiers and associated metadata. While in an iBeacon region, Proximity Kit provides additional ranging services for continuous proximity updates relative to the phone's distance from the iBeacon.

---

# SDK Download and Install

### iOS

Download and install the configuration file and library.

<a class="btn" href="http://proximitykit.com/download">iOS SDK Download and Install</a>
<a class="btn" href="http://proximitykit.com/android-download">Android SDK Download and Install</a>

## Getting Started

Quick start guide to adding Proximity Kit to your iOS application.

<a class="btn" href="gettingstarted">Getting Started Guide</a>

## Example App

A simple example of using Proximity Kit in an App.

<a class="btn" href="https://github.com/RadiusNetworks/proximity-kit-ios-example">iOS Example App on GitHub</a>

