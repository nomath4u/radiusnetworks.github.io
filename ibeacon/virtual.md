---
layout: ibeacon
---

# Virtual iBeacon

Create a VM that will work like an iBeacon.

## What you need

1. Virtual Machine software, either VMWare or [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
2. A Low Energy Bluetooth USB Dongle, [selling for about $12](http://www.amazon.com/dp/B007GFX0PY/ref=pe_385040_30332190_pe_175190_21431760_M3T1_ST1_dp_1).  A full list of those known to work and not work is shown below.

## How to set it up on VirtualBox

1. Ensure your Virtual Machine software is properly installed
2. Download the virtual machine [here](https://s3.amazonaws.com/s3.messageradius.com/Public/Virtual_iBeacon.ova).
3. In VirtualBox, choose File -> Import Appliance, select the downloaded ova file, and follow the prompts to set it up.
4. Plug in your Bluetooth LE Dongle
5. Start the virtual machine.  The Virtual iBeacon will attempt to startup automatically.
6. If you receive a message in the virtual machine that no bluetooth device has been detected,  make sure the virtual machine has captured the device.  In the VirtualBox menu, select Devices -> USB Devices -> then select your BLE USB device in the menu that shows up so that there is a checkmark by it.
     *  If you receive an error message trying to capture your bluetooth device (it will look like this)
      <img style="height: 200px; margin: 10px 30px 20px 0; border: 2px solid #f5f5f5; float:left; border-radius: 7px;" src='http://i.imgur.com/qzMirYi.png'>
        
     A quick solution to this problem on OSX computers can be found [here](https://www.virtualbox.org/ticket/2372#comment:12)

7. Now you can start the Virtual iBeacon manually by typing `start` into the command prompt and pressing enter.



## How to start it and stop it

To start the Virtual iBeacon, type: `start` into the command prompt and press enter.
To stop the Virtual iBeacon, type: `stop` and press enter.

## How to tell if it is working

Out of the box, the Virtual iBeacon sends out an advertisement once per second with the same proximityUUID used on Apple's AirLocate test app E2C56DB5-DFFB-48D2-B060-D0F5A71096E0 with a major of 1 and a minor of 1.

If you have a copy of Radius Networks' free iBeacon Locate app for [iOS](https://itunes.apple.com/us/app/ibeacon-locate/id738709014) or [Android](https://play.google.com/store/apps/details?id=com.radiusnetworks.ibeaconlocate&hl=en), launch it and tap Locate iBeacons.  You should see regular updates from the Virtual iBeacon.  
## How to configure it

You can set the three broadcast identifiers (UUID, MAJOR, MINOR) by editing the ibeacon.conf file.  Note that these identifiers must be entered as individual hexadecimal bytes separated by spaces.  For UUID, this means you don't type any dashes and put a space between each two characters.  For MAJOR and MINOR, numbers must be entered as two byte hexadecimal numbers, the most significant byte first.

If you don't know hex, you can use [this site](http://www.binaryhexconverter.com/decimal-to-hex-converter) to convert numbers for you.  Just know that you need to take the result and turn it into two bytes separated by spaces.  So for a decimal value of 12, you put the "C" hex result into the config file as "00 0C".  For the decimal value of 12345, you put the "3039" result into the config file as "30 39".

You can also set the POWER measurement in the config file.  This is the calibrated Transmitter power, and the value you set is specific to your Bluetooth LE dongle.  Setting this is optional (a default value has been programmed), but if you don't configure it for your BLE device, you may get less accurate distance measurements.  Calibration involves using the AirLocate app (iOS) or IBeaconLocate (Android) calibration feature to measure the average RSSI of the beacon when the phone is held one meter away.  The resulting value is a negative number that must be encoded as hex.  You can do the hex encoding using the same converter as above, but in this case you use only the last two characters of the hex value.  So if the calibrated Tx Power is -98 RSSI, the converted hex value is "FFFFFFFFFFFFFF9E" and you would enter "9E" into the config file for POWER.

## Bluetooth LE Hardware Devices

For a BLE Device to work with the Virtual Beacon, it must have low level USB compatibility with your host computer as well as compatibility with the Linux Bluez Bluetooth stack.  A Full list of devices known to work and not work are shown below.  Please send us reports of other working hardware to jnebeker _at_ radiusnetworks _dot_ com.

###Known to work:

* [IOGEAR Bluetooth 4.0 USB Micro Adapter GBU521](http://www.amazon.com/dp/B007GFX0PY/ref=pe_385040_30332190_pe_175190_21431760_M3T1_ST1_dp_1) (Broadcomm BCM20702A0 Chip set)
* [Plugable USB-BT4LE Bluetooth 4.0 USB Adapter for Windows and Linux PCs](http://plugable.com/products/usb-bt4le)

###Reported to work with modifications:
* Belkin Bluetooth v.4.0 adapter (Broadcom BCM20702 Chip set)  [See Modifications](#BCM20702)

###Known not to work:

* Cambridge Semiconductor 8510 A10 (incompatible with Bluez stack)
* TI CC2540 USB Dongle (does not work with Bluez stack in default configuration)
* Internal MacBook Pro Bluetooth LE device (cannot be captured by Virtual Machine)

## Questions or Issues?

Contact jnebeker _at_ radiusnetworks _dot_ com if you have any questions or need more information.


## Notes for specific devices

Note: These modifications have been reported by end-users and have not been verified by Radius Networks.

1. <a name='BCM20702'>BCM20702</a>

```
Inspect /var/adm/kernel.log you will find something similar to

P:  Vendor=0489 ProdID=e031 Rev=01.12
S:  Manufacturer=Broadcom Corp
S:  Product=BCM20702A0
S:  SerialNumber=3859F9CD2AEE

Execute the following:

$ modprobe btusb
$ echo "0489 e031" >> /sys/bus/usb/drivers/btusb/new_id
 
Adjust the values in quotes above to match the Vendor and ProdID you find in the log file.

```


