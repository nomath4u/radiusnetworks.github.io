---
layout: post
title: Announcing iBeacon Development Kit Version 2.0
author: James Nebeker
---

We are excited to announce an update to our top-selling [iBeacon Development Kit](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit.html).
With version 2.0, we made the kit more user friendly and added new features that make it even more valuable in the development of iBeacon enabled apps.  This post will outline these new features and how they can be helpful as well as instruct you on how to upgrade your existing IDK to the new version.

# What’s New in Version 2.0?

The iBeacon Development Kit is now much easier to use and configure.  As before, you can easily configure the iBeacon identifiers by inserting the SD card into your laptop.  Now, instead of having to convert these values into hexadecimal form you can simply copy and paste your UUID in the standard format and enter all other values in plain decimal format.  We’ve also made controlling the iBeacon from the console much easier with simple commands for start and stop and a help command to explain everything (see the [instructions page](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit-instructions.html) for more info).  All in all, you’ll find that the latest version lets you get to developing much faster and with less hassle.  

Also new in the latest version is the ability to customize the iBeacon advertising frequency.  The responsiveness of an iBeacon enabled app depends heavily on how often the iBeacon being detected by the app is broadcasting.  The higher the frequency the more responsive the app will be when being triggered.  However, higher frequency can mean a shorter battery life for iBeacons running on battery power.  Customizable broadcast frequency give you the ability to test and design an app for all types of iBeacons, whether they’re designed for high performance or power-saving.

Additionally, all iBeacon Development Kits now ship with a default advertising rate of 10 times per second instead of the previous 1 time per second.  This is the recommended value from Apple engineers and will allow much more responsive triggering of a mobile device.

# How to Upgrade

If you’ve purchased the previous version of the iBeacon Development Kit, or if you already have your own Raspberry Pi, bluetooth dongle, and a spare SD card, we’ve made the update available to download for free.  To upgade, first download the disk image for version 2.0 [here](https://s3.amazonaws.com/s3.messageradius.com/Public/IDK.gz).  Next, insert the SD card from your old development kit into your computer.  Now you will need to uncompress the downloaded disk image file and write it onto the SD card.  For Windows systems this can be accomplished with [Win32DiskImager](http://sourceforge.net/projects/win32diskimager/), for OS X and Linux you can use command line tools.  Instructions for this are given below:

## Windows

1. Head to the start menu or screen and type "disk management." Open the disk management program and find your SD card in the list.
1. Right-click and delete all the partitions on your SD card. When it's empty, right-click on it and format it (it doesn't matter what filesystem you format it to, your computer just needs to recognize it).
1. Open Win32DiskImager and browse for the downloaded, uncompressed image file. Select the SD card from the “Device” dropdown.
1. Click "Write" to write the image to the SD card.
1. When it finishes, eject the SD card and re-insert it into your Raspberry Pi. Now you’re upgraded to version 2.0!

## OS X

1. Open terminal and run the `diskutil list` command to identify your SD card, you should see something like this.

 ```	
 $ diskutil list
 /dev/disk0
    #:                       TYPE NAME                    SIZE       IDENTIFIER
    0:      GUID_partition_scheme                        *500.1 GB   disk0
    1:                        EFI                         209.7 MB   disk0s1
    2:                  Apple_HFS Macintosh HD            499.2 GB   disk0s2
    3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
 /dev/disk1
    #:                       TYPE NAME                    SIZE       IDENTIFIER
    0:     FDisk_partition_scheme                        *7.9 GB     disk1
    1:             Windows_FAT_32                         58.7 MB    disk1s1
    2:                      Linux                         7.9 GB     disk1s2
 ```

 In this example, `/dev/disk1` is the 8GB SD card.  

2. Now use the `dd` command combined with the `gzip` command (see below) to unzip and write the disk image to the SD card.  You will need the full path to the disk image file and you should use `/dev/rdiskX` instead of `/dev/diskX`.  Note: it is very important that you enter the correct disk into the command, otherwise you could corrupt your computer’s hard disk, so be sure to double check before you press enter.  

`gzip -dc /path/to/IDK.gz | sudo dd of=/dev/rdisk1 bs=1m`

3. It should take a while for the command to finish, when it’s done you’ll see the command prompt return.  After it finishes, eject the SD card and re-insert it into your Raspberry Pi. Now you’re upgraded to version 2.0!

Once you update your kit be sure to read the updated [instructions](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit-instructions.html) to learn more about the new features and how to use them, happy developing!


More information on these steps can be found [here](http://lifehacker.com/how-to-clone-your-raspberry-pi-sd-card-for-super-easy-r-1261113524) and [here](http://raspberrypi.stackexchange.com/questions/311/how-do-i-backup-my-raspberry-pi).
