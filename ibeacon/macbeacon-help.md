---
layout: ibeacon
---

# Can my Mac run MacBeacon?

This depends entirely on the bluetooth module that the computer has. You can use System Profiler to determine if you can use the built in Bluetooth module or if you need to use an external USB module.

The version can be checked by clicking on the Apple Menu in the Menu Bar and following: `About this Mac > More Info > System Report > Hardware > Bluetooth`

<img style="width:630px" src="/img/macbeacon-help-lme-version.png" />

Then you should see LMP Version in the list, which works out to be a funny hex representation of the Bluetooth Version.

* LMP Version 0x4 is Bluetooth Core Specification 2.1 + EDR
* LMP Version 0x6 is Bluetooth Core Specification 4.0

You need to have a version of 0x6 (Bluetooth Core Specification 4.0, e.g. BTLE), otherwise you will need to get a BTLE USB Stick.

# How do I use an External USB Bluetooth Module?

If your Mac does not support BTLE you can use an external USB Bluetooth Module.

Luckily that is pretty easy to resolve with an inexpensive external Bluetooth device like [IOGEAR Bluetooth 4.0 USB Micro Adapter](http://www.amazon.com/dp/B007GFX0PY/ref=pe_385040_30332190_pe_175190_21431760_M3T1_ST1_dp_1)

When you install the external Bluetooth device, you may need to force the system to be able to recognize it and switch the Bluetooth stack to using it. The way to do that is to run the following command in terminal:

```
sudo nvram bluetoothHostControllerSwitchBehavior="always"
```

You may need to remove and reinsert the USB module or restart your computer. To verify that you are using the new Bluetooth hardware, check your system information and make sure that your LMP-version is 0x6.
