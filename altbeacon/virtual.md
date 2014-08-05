---
layout: content
---

# Virtual AltBeacon

Create a VM that transmits as a proximity beacon with [AltBeacon](http://altbeacon.org) technology and scans for other nearby beacons with the AltBeacon specification.

<a class="btn" href="https://s3.amazonaws.com/s3.radiusnetworks.com/Public/Virtual_AltBeacon.ova">Download Virtual AltBeacon</a>


## What you need

1. Virtual Machine software, either [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or VMWare.
2. A Low Energy Bluetooth USB Adapter, [selling for about $12](http://www.amazon.com/dp/B007GFX0PY/ref=pe_385040_30332190_pe_175190_21431760_M3T1_ST1_dp_1).  A full list of those known to work and not work is shown below.

## How to set it up on VirtualBox

1. Ensure your Virtual Machine software is properly installed and you have the latest version.
2. Download the virtual machine file with the link above.
3. In VirtualBox, choose File -> Import Appliance, select the downloaded ova file, and follow the prompts to set it up.
4. Plug in the Bluetooth LE USB adapter to your host machine
5. Start the virtual machine.  The Virtual AltBeacon will attempt to begin transmitting automatically.
6. If you receive a message in the virtual machine that no bluetooth device has been detected,  make sure the virtual machine has captured the device.  In the VirtualBox menu, select Devices -> USB Devices -> then select your BLE USB device in the drop-down menu so that there is a checkmark by it.
     *  If you receive an error message like the one below trying to capture your bluetooth device, a quick solution to this problem is outlined in a [section below](#USBERROR).

        <img style="height: 125px; margin: 30px 30px 20px 0; border: 2px solid #f5f5f5; border-radius: 7px;"             src='http://i.imgur.com/qzMirYi.png'>

7. Now you can use the Virtual AltBeacon manually by calling the altbeacon scripts in the command prompt and pressing enter.

```
altbeacon_transmit
 altbeacon_stop
 altbeacon_receive
```

## How to tell if it is working

Out of the box, the Virtual AltBeacon sends out an advertisement once per second with null ID1 (00000000-0000-0000-0000-000000000000), ID2 of 1, and ID3 of 1.  Visit the [AltBeacon examples](http://altbeacon.org/examples/) page for tools to detect the Virtual AltBeacon and confirm it is transmitting.

## Customizing Identifiers

You can set the three broadcast identifiers (ID1, ID2, ID3) by editing the `altbeacon.conf` file.  Note that these identifiers must be entered as individual hexadecimal bytes separated by spaces.  For ID1, this means you don't type any dashes and put a space between each set of two characters.  For ID2 and ID3, numbers must be entered as two byte hexadecimal numbers, the most significant byte first.

If you don't know hex, you can use [this site](http://www.binaryhexconverter.com/decimal-to-hex-converter) to convert numbers for you.  Just know that you need to take the result and turn it into two bytes separated by spaces.  So for a decimal value of 12, you put the "C" hex result into the config file as "00 0C".  For the decimal value of 12345, you put the "3039" result into the config file as "30 39".

You can also set the Reference_RSSI measurement in the config file.  This is the calibrated Transmitter power, and the value you set is specific to your Bluetooth LE dongle.  Setting this is optional (a default value has been programmed), but if you don't configure it for your BLE device, you may get less accurate distance measurements.  Calibration involves using the AltBeacon Locate (Android) calibration feature to measure the average RSSI of the beacon when the phone is held one meter away.  The resulting value is a negative number that must be encoded as hex.  You can do the hex encoding using the same converter as above, but in this case you use only the last two characters of the hex value.  So if the calibrated Tx Power is -98 RSSI, the converted hex value is "FFFFFFFFFFFFFF9E" and you would enter "9E" into the config file for Reference RSSI.

## Bluetooth LE Hardware Devices

For a BLE Device to work with the Virtual AltBeacon, it must have low level USB compatibility with your host computer as well as compatibility with the Linux BlueZ Bluetooth stack.  A full list of devices known to work and not work are shown below.  Please send us reports of other working hardware to support _at_ radiusnetworks _dot_ com.

###Known to work:

* [IOGEAR Bluetooth 4.0 USB Micro Adapter GBU521](http://www.amazon.com/dp/B007GFX0PY/ref=pe_385040_30332190_pe_175190_21431760_M3T1_ST1_dp_1) (Broadcomm BCM20702A0 Chip set)
* [Plugable USB-BT4LE Bluetooth 4.0 USB Adapter for Windows and Linux PCs](http://plugable.com/products/usb-bt4le)

###Reported to work with modifications:
* Belkin Bluetooth v.4.0 adapter (Broadcom BCM20702 Chip set)  [See Modifications](#BCM20702)

###Known not to work:

* Cambridge Semiconductor 8510 A10 (incompatible with Bluez stack)
* TI CC2540 USB Dongle (does not work with Bluez stack in default configuration)
* Internal MacBook Pro Bluetooth LE device (cannot be captured by Virtual Machine)

## <a name='USBERROR'>Bluetooth Device Capture Error</a>

Below is an outline for a fix to the common error where the host machine captures the bluetooth dongle before it can be captured by the virtual machine:

* Make sure you have the latest version of VirtualBox.
* Before you launch the virtual machine, plug in your bluetooth USB dongle and set up a filter so your VM will capture it    automatically (Settings --> Ports --> USB).  It should look like this:
    <img style="height: 300px; margin: 10px 30px 20px 0; border: 2px solid #f5f5f5; border-radius: 7px;"             src='http://i.imgur.com/DlY9dkk.png'>
* Exit settings and unplug the Bluetooth adapter from your computer.
* Now launch your VM and wait until it boots to plug in the Bluetooth adapter.
* After you plug in the adapter, the VM should capture it automatically.  To verify this, type `hciconfig` into the command line and you should see a `hci0` device listed.
* Now you should be able to type `altbeacon_transmit` to begin broadcasting as an AltBeacon.

If this doesn't work, you can also try another fix (for OS X) found [here](https://www.virtualbox.org/ticket/2372#comment:12)


## Other Questions or Issues?

Contact support _at_ radiusnetworks _dot_ com if you have any questions or need more information.


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
