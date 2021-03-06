---
layout: content
---

# Updating Your PiBeacon

If you’ve purchased a previous version of the PiBeacon, its precursor the Beacon Development Kit, or if you already have your own Raspberry Pi, 
bluetooth dongle, and a spare SD card, we’ve made the PiBeacon software available as a Raspberry Pi disk 
image, downloadable for free. To upgrade, first download the disk image for the latest version below (currently Version 4.0). Next, uncompress the downloaded zip file; the uncompressed file should be titled 'PiBeacon-4.0.img'.
Now insert the old SD card into your computer and follow the steps below to write the image file onto the SD card.  For Windows systems this can be accomplished with 
[Win32DiskImager](http://sourceforge.net/projects/win32diskimager/).  For OS X and Linux you can use command line tools. 

<a class="btn" href="https://s3.amazonaws.com/s3.radiusnetworks.com/Public/PiBeacon-4.0.img.zip">Download Disk Image</a>

## Windows

1. Head to the start menu or screen and type "disk management." Open the disk management program and find your SD card in the list.

2. Right-click and delete all the partitions on your SD card. When it's empty, right-click on it and format it (it doesn't matter what filesystem you format it to, your computer just needs to recognize it).  To make the newly formatted filesystem recognizable you may need to perform the "Change Drive Letter and Paths..." option and assign it a letter.  

3. Since you're formatting an old Raspberry Pi SD card, Windows will not be able to recognize the Linux partition and thus can't reformat the entire card.  Instead, you can reformat it completely with [SD Formatter 4.0](https://www.sdcard.org/downloads/formatter_4/), make sure the "FORMAT SIZE ADJUSTMENT" option is on.

4. Once the SD card is fully formatted for Windows, open Win32DiskImager and browse for the downloaded, uncompressed disk image file. Select your SD card from the “Device” dropdown.

5. Click "Write" to write the image to the SD card.

6. When it finishes, eject the SD card and re-insert it into your Raspberry Pi. Now you’re upgraded to the latest version!


## OS X

1. Open terminal and run the `diskutil list` command to identify your SD card, you should see something like this.    In this example, `/dev/disk1` is the 8GB SD card (note the 'Linux' partition).  


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

2. Now use the `dd` command (see below) to write the disk image to the SD card.  You will need the full path to the uncompressed disk image file and you should use `/dev/rdiskX` instead of `/dev/diskX`(where X is the number corresponding to your SD card). If you get a "Resource busy" error you need to unmount any partitions on the SD card with Disk Utility.  <div style="color: red;">**IMPORTANT: it is CRITICAL that you enter the correct disk into this command, otherwise you could corrupt your computer’s hard disk.  Be sure to double check the disk before you press enter!**</div>

       ```    
       sudo dd if=/path/to/PiBeacon-4.0.img of=/dev/rdiskX bs=1m
       ```

3. It should take a while for the command to finish, when it’s done you’ll see the command prompt return.  After it finishes, eject the SD card and re-insert it into your Raspberry Pi. Now you’re upgraded to the latest version!

Once you update your PiBeacon, be sure to read the updated [instructions](http://developer.radiusnetworks.com/pibeacon/pibeacon-instructions.html) to learn more about the new features and how to use them.

More information on these steps can be found [here](http://lifehacker.com/how-to-clone-your-raspberry-pi-sd-card-for-super-easy-r-1261113524) and [here](http://raspberrypi.stackexchange.com/questions/311/how-do-i-backup-my-raspberry-pi).
