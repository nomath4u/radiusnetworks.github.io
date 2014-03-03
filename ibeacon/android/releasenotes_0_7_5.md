---
layout: android-ibeacon-library
---


###Release 0.7.5

* Tighten up cleanup during iBeaconManager.unBind in attempt to address reports of service not stopping properly. https://github.com/RadiusNetworks/android-ibeacon-service/issues/21
* Fix synchonization exceptions when regions are dynamically modifie by keeping track of regions in manager
* Fix setting foreground scan period per https://github.com/RadiusNetworks/android-ibeacon-service/issues/17
* Change method instructions and signature for updating scan periods.
