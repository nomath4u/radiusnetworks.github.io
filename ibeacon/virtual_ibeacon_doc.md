---
layout: ibeacon
---

## Virtual iBeacon Documentation

Radius Networks' Virtual iBeacon is a small virtual machine image you can use as a development iBeacon on Mac Windows and Linux computers.  Best of all it is available for FREE!

### What you need

1. Virtual Machine software, either VMWare or [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
2. A Low Energy Bluetooth USB Dongle, [selling for about $12](http://www.amazon.com/dp/B007GFX0PY/ref=pe_385040_30332190_pe_175190_21431760_M3T1_ST1_dp_1).  A full list of those known to work and not work is shown below.

### How to set it up on VirtualBox

1. Ensure your Virtual Machine software is properly installed
2. Download the virtual machine [here](https://s3.amazonaws.com/s3.messageradius.com/Public/VirtualiBeacon.ova).
3. In VirtualBox, choose File -> Import Appliance, select the downloaded ova file, and follow the prompts to set it up.
4. Start the virtual machine.  In the black window, you will see the machine boot up and give you a login prompt.
5. Log in with the credentials username: user, password: password
6. Plug in your Bluetooth LE Dongle
7. Make sure the virtual machine has captured the device.  In the VirtualBox menu, select Devices -> USB Devices -> then select your BLE USB device in the menu that shows up so that there is a checkmark by it.
8. Verify the BLE device is available to the Virtual Beacon.  At the command prompt, type: `sudo hciconfig` and enter the password 'password' when prompted.   If you see a response with 'hci0' in it, you are ready to go.  If you see no response, then that means the USB device has not been captured by the virtual machine, or it is incompatible with your host computer or the Linux Bluez stack.

### How to set it up on VMWare Fusion
VMWare products generally support that ability to import an Open Virtualization Format (.ovf and .ova) virtual machine and run it in VMWare Fusion. These instructions apply specifically to VMWare Fusion, but should be similar in other VMWare products.

1. Ensure your VMWare software is properly installed
2. Download the virtual machine [here](https://s3.amazonaws.com/s3.messageradius.com/Public/VirtualiBeacon.ova).
3. From the VMware Fusion menu bar, select File > Import. The Import Library window appears, along with a dialog box for browsing to the location of OVF file.
4. Browse to the .ova file and click Open. Type the name for the imported virtual machine in the Save As text box and indicate where to save it. The default destination is the Virtual Machines folder created by VMware Fusion. VMware Fusion displays the disk space needed for the import, and the space available on the current disk.
5. Click Import. Fusion performs OVF specification conformance and virtual hardware compliance checks. A status bar indicates the progress of the import process. After the import is complete, the virtual machine appears in the virtual machine library and in a separate virtual machine window. The virtual machine is shut down.
5. Start the virtual machine.  In the black window, you will see the machine boot up and give you a login prompt.
6. Log in with the credentials: username: user, password: password
7. Plug in your Bluetooth LE Dongle
8. Make sure the virtual machine has captured the device.  In the VirtualBox menu, select Devices -> USB Devices -> then select your BLE USB device in the menu that shows up so that there is a checkmark by it.
9. Verify the BLE device is available to the Virtual Beacon.  At the command prompt, type: `sudo hciconfig` and enter the password 'password' when prompted.   If you see a response with 'hci0' in it, you are ready to go.  If you see no response, then that means the USB device has not been captured by the virtual machine, or it is incompatible with your host computer or the Linux Bluez stack.

### How to start it and stop it

To start the Virtual iBeacon, type: `./start` (enter the password of 'password' if prompted)
To stop the Virtual iBeacon, type: `./stop` 

### How to tell if it is working

Out of the box, the Virtual iBeacon sends out an advertisement once per second with the same proximityUUID used on Apple's AirLocate test app E2C56DB5-DFFB-48D2-B060-D0F5A71096E0 with a major of 1 and a minor of 1.

If you have a copy of Apple's AirLocate iOS test app, launch it and start it ranging.  You should see regular updates from the Virtual iBeacon.  If you have an Android device with Bluetooth LE, you can use Radius Networks' IBeaconLocate test app to do the same thing.

### How to configure it

You can set the three broadcast identifiers (UUID, MAJOR, MINOR) by editing the ibeacon.conf file.  Note that these identifiers must be entered as individual hexadecimal bytes separated by spaces.  For UUID, this means you don't type any dashes and put a space between each two characters.  For MAJOR and MINOR, numbers must be entered as two byte hexadecimal numbers, the most significant byte first.  

If you don't know hex, you can use [this site](http://www.binaryhexconverter.com/decimal-to-hex-converter) to convert numbers for you.  Just know that you need to take the result and turn it into two bytes separated by spaces.  So for a decimal value of 12, you put the "C" hex result into the config file as "00 0C".  For the decimal value of 12345, you put the "3039" result into the config file as "30 39".

You can also set the POWER measurement in the config file.  This is the calibrated Transmitter power, and the value you set is specific to your Bluetooth LE dongle.  Setting this is optional, but if you don't configure it, you may get less accurate distance measurements.  Calibration involves using the AirLocate app (iOS) or IBeaconLocate (Android) calibration feature to measure the average RSSI of the beacon when the phone is held one meter away.  The resulting value is a negative number that must be encoded as hex.  You can do the hex encoding using the same converter as above, but in this case you use only the last two characters of the hex value.  So if the calibrated Tx Power is -98 RSSI, the converted hex value is "FFFFFFFFFFFFFF9E" and you would enter "9E" into the config file for POWER.

### Bluetooth LE Hardware Devices

For a BLE Device to work with the Virtual Beacon, it must have low level USB compatibility with your host computer as well as compatibility with the Linux Bluez Bluetooth stack.  A Full list of devices known to work and not work are shown below.  Please send us reports of other working hardware to james _at_ radiusnetworks _dot_ com.

#### Known to work:

* IOGEAR Bluetooth 4.0 USB Micro Adapter (GBU521)

#### Known not to work:

* Cambridge Semiconductor 8510 A10 (incompatible with Bluez stack)
* TI CC2540 USB Dongle (does not work with Bluez stack in default configuration)
* Internal MacBook Pro Bluetooth LE device (cannot be captured by Virtual Machine)