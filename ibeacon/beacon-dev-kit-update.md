---
layout: ibeacon
---

# Updating Your Beacon Development Kit

If you’ve purchased a previous version of the Beacon Development Kit, or if you already have your own Raspberry Pi, 
bluetooth dongle, and a spare SD card, we’ve made the Beacon Development Kit software available as a Raspberry Pi disk 
image, downloadable for free. To upgrade, first download the disk image for the latest version 
[here](https://s3.amazonaws.com/s3.radiusnetworks.com/Public/IDK3-1.gz) (currently Version 3.1). 
Next, insert the SD card from your old development kit into your computer. Now you will need to uncompress the 
downloaded disk image file and write it onto the SD card. For Windows systems this can be accomplished with 
[Win32DiskImager](http://sourceforge.net/projects/win32diskimager/), 
for OSX and Linux you can use command line tools. Instructions for these are given below:

##Windows

1. Head to the start menu or screen and type "disk management." Open the disk management program and find your SD card in the list.

2. Right-click and delete all the partitions on your SD card. When it's empty, right-click on it and format it (it doesn't matter what filesystem you format it to, your computer just needs to recognize it).

3. Open Win32DiskImager and browse for the downloaded, uncompressed disk image file. Select the SD card from the “Device” dropdown.

4. Click "Write" to write the image to the SD card.

5. When it finishes, eject the SD card and re-insert it into your Raspberry Pi. Now you’re upgraded to the latest version!


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

2. Now use the `dd` command combined with the `gzip` command (see below) to unzip and write the disk image to the SD card.  You will need the full path to the disk image file and you should use `/dev/rdiskX` instead of `/dev/diskX`. <div style="color: red;"><div style="font-weight: bold;">IMPORTANT: it is CRITICAL that you enter the correct disk into this command, otherwise you could corrupt your computer’s hard disk.  Be sure to double check the disk before you press enter!  </div></div>

 `gzip -dc /path/to/IDK.gz | sudo dd of=/dev/rdiskX bs=1m`

3. It should take a while for the command to finish, when it’s done you’ll see the command prompt return.  After it finishes, eject the SD card and re-insert it into your Raspberry Pi. Now you’re upgraded to version 3.0!

Once you update your kit be sure to read the updated [instructions](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit-instructions.html) to learn more about the new features and how to use them, happy developing!

More information on these steps can be found [here](http://lifehacker.com/how-to-clone-your-raspberry-pi-sd-card-for-super-easy-r-1261113524) and [here](http://raspberrypi.stackexchange.com/questions/311/how-do-i-backup-my-raspberry-pi).

Note: You may experience some instability when using the new scan feature for extended periods of time in high-traffic areas due to an error where the bluetooth adapter enters a bad state.  The only way to recover from this state is to power cycle the adapter or reboot the machine.  Read the [instructions](http://developer.radiusnetworks.com/ibeacon/ibeacon-development-kit-instructions.html) page for some tips on preventing this error and improving scanning stability.

