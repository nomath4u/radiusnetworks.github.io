---
layout: content
---

# PiBeacon Instructions (Version 4.0)

##Getting Started

The PiBeacon consists of a Raspberry Pi computer, Bluetooth Low Energy (BLE) USB adapter, and an SD card preloaded with BLE beacon utility software.  It requires a standard 5V power supply with a micro USB connector.  There is no power switch for the PiBeacon.  As soon as it is plugged in, it will boot up and begin transmitting automatically after about a minute.

Note: The Raspberry Pi can be sensitive to poorly performing chargers. If your PiBeacon doesn’t boot properly (you should see a red LED illuminated and a green LED that flashes intermittently) try a different charger.

To verify the PiBeacon is transmitting, use a beacon test tool like Radius Networks’ “Locate Beacon” (iOS). [Click here](http://store.radiusnetworks.com/collections/all) to check out more proximity beacon tools from Radius Networks. 

## PiBeacon Flexibility

The PiBeacon is the ultimate BLE beacon tool, it has the ability to transmit with both the iBeacon and AltBeacon standards with fully customizable identifiers and transmit frequency.  In addition, it can scan for other nearby beacons of both standards.  For more information on AltBeacon (the open and interoperable proximity beacon specification) visit AltBeacon.org.

## Initial PiBeacon Configuration

The PiBeacon will begin transmitting at boot with the default identifiers, these can be easily configured by editing the `/boot/beacon.conf` file on the PiBeacon.  This can be done easily by removing the SD card and placing it in your computer’s card reader slot.  Alternatively, you can edit the file directly on the Pi with either a keyboard and monitor or via SSH.  When you open this file, you should see two blocks like the one below, these give the parameters and identifiers for each transmitter (Note: changing the second set of identifiers does nothing unless you have the dual model).

```    
export BEACON_TYPE_1=iBeacon
export ID1_UUID_1=2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6
export ID2_MAJOR_1=1
export ID3_MINOR_1=1
export RESERVED_1=0
export POWER_1=-59
export FREQUENCY_1=10
```
 Once you select your beacon type, identifiers, and frequency be sure to 
properly save the file and eject it from your computer.  Note: if you change to a custom UUID you will need to add this UUID to your ‘Locate Beacon’ iOS app 
for it to be visible (iOS devices can only detect beacons with known UUIDs).

## Controlling the PiBeacon

The PiBeacon’s beacon capabilities are powered by the BLE beacon command line utility developed by Radius Networks, which can be used to transmit and scan for BLE proximity beacons.  To use this tool, you need access to a terminal in the Raspberry Pi.  You can do this by attaching a monitor and keyboard to the Raspberry Pi (before you power it on) or by connecting the Pi to your local network and accessing it from another computer via SSH (see the section on connecting via SSH for more info).  Log into the Raspberry Pi with the default credentials: username: pi, password: raspberry.  Now you can use the `beacon` command to control your PiBeacon.  Run the command with the help option to see the proper usage:
```
beacon --help
```
The utility is broken up into different commands that control the different functions of the PiBeacon.  The following sections describe the details of these commands.

### Transmit

Use this command to transmit as a BLE beacon.  You are required to specify a beacon type (AltBeacon or iBeacon).  Optionally, you can include other parameters and identifiers to be transmitted.  For example, the following command will transmit with the AltBeacon standard at 5 Hz with null identifiers:

```
pi@pibeacon ~ $ sudo beacon transmit -A -f 5 -1 00000000-0000-0000-0000-000000000000 -2 0 -3 0 -r 0
Transmitting with AltBeacon standard at 5.00 Hz
ID1: 00000000-0000-0000-0000-000000000000, ID2: 0, ID3: 0, Power: -59, Mfg Reserved: 0
```
To transmit with different parameters, simply rerun the transmit command with your desired settings.

### Stop

This command will simply stop the PiBeacon from transmitting as a BLE beacon.  Until it receives a stop command or it loses power, the PiBeacon will transmit indefinitely once activated.

### Scan

Using the scan command, the PiBeacon will scan for other nearby BLE beacons and list the information of every transmission it receives.  You can select which type of beacon to scan for (it will scan for both AltBeacon and iBeacon standards by default) as well as set the duration of the scan (if desired).  In addition, you can select the output format to easily parse the scan data in another program.

### BlueZ

The BLE beacon tool uses BlueZ, the official Linux Bluetooth protocol stack, to communicate with the Bluetooth adapter.  There are a couple BlueZ commands that are useful when diagnosing problems with the PiBeacon.  First the `hciconfig` command will list the information and status of any attached Bluetooth devices.  A device must be in the “up” state to execute any Bluetooth commands.  To bring a device up use the following command (device “hci0” in this case):
```
sudo hciconfig hci0 up
```
To reset a Bluetooth device, which can fix any unexpected behaviors, use the following command:
```
sudo hciconfig hci0 reset
```
### Dual Model Commands

If you have the dual model of the PiBeacon, you can specify beacon commands on each device independently with the `-i` option.  For example, the following commands will transmit with one device and scan with another:
```
pi@pibeacon ~ $ sudo beacon -i hci0 transmit -A
Transmitting with AltBeacon standard at 10.00 Hz
ID1: 2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6, ID2: 1, ID3: 1, Power: -59, Mfg Reserved: 0
pi@pibeacon ~ $ sudo beacon -i hci1 scan -A
ID1:  2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6  ID2:  1  ID3:  1  POWER:  -69  RSSI:  -82  RESERVED:  0
```

### Connecting via SSH

To connect to the PiBeacon using SSH, you will need basic Linux command line skills, an ethernet cable, and access to a router.

Step 1. Using an ethernet cable, plug one end into the ethernet port on the Raspberry Pi computer and the other end into your router.

Step 2. Power on the PiBeacon.

Step 3. Using a computer, go to your router’s administration page and determine the IP address of the PiBeacon.  It should show up on your router’s attached devices list as “pibeacon”

Step 4. On your computer connected to the same router, open up a ssh client and connect to the PiBeacon, logging in with username: pi, password: raspberry.  For example, on a Linux or OS X machine use the ssh command (substitute your PiBeacon’s IP address):
```
ssh pi@192.168.1.10
```


## Next Steps

Now you can have the tools to start developing a proximity aware mobile app or use the power of the Raspberry Pi for proximity aware automation. Visit our website to check out tools and other products and check out our developer blog for more tutorials with the Raspberry Pi.

##Getting Help 

If you have problems configuring or operating the PiBeacon, email us at support@radiusnetworks.com
