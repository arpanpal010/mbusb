mbusb
=====
multiboot usb mainly for distro hopping and easy installations, but contains lines to boot sysadmin tools too.

####List of supported OS(s):####
* Android
  * Android 4.4 RC2

* Linux
  * Arch Linux
  * GRML
  * Kali Linux
  * Linux Mint
  * Puppy Slacko
  * Tails liveCD

* Tools
  * SystemRescueCD
  * GParted Live
  * Clonezilla
  * Memtest86+
  * DBAN

Description
------------

This is a method to boot multiple OS(s) from a single usb drive without ever needing to write any CD or creating separate USB keys for each OS, all we need is a vFAT(FAT32) formatted USB drive. 

Currently using grub2 as the bootloader, and booting the iso files with loopback.  
x86_64 OSs will be hidden by default if the system does not support them.

Installation
---------------
First, the USB has to be made bootable, so
After inserting USB, do
```
sudo fdisk -l
```
and note down the device id of the USB, in our case its /dev/sdb, and the partition is /dev/sdb1. Now, format the partition as vfat by typing
```
sudo mkfs.vfat /dev/sdb1
```
(if there's no .vfat option, the `dosfstools` package is probably not installed.)
mount the USB in /mnt/
```
sudo mount /dev/sdb1 /mnt/
```
and make a directory called `boot` in /mnt
```
sudo mkdir /mnt/boot
```
Now to install the bootloader,i.e grub2, enter (be sure to write **/dev/sdb** as we're installing grub in the drive, not the partition)
```
sudo grub-install --boot-directory=/mnt/boot /dev/sdb
```
Now you can unmount /mnt with
```
sudo umount /mnt
```
After that, mount the drive as a user, clone/download-as-zip this repo somewhere in your storage, and copy the files (after unzipping for the latter) over to the /boot/ folder of the USB. Replace the existing files if there are any conflicts (mostly themes).

The iso files go into /boot/iso/, and if the .iso comes in two versions separately (x86 and x86_64),they should be in their own subdirectory (e.g *Linux Mint*, *Kali Linux* etc..) check the `/boot/iso/hierarchy.txt` if you have any confusion about filenames or locations.
 
Now remove the USB and put it to good use. Use it just to run Live OS(s) or Install in computers, the possibilities are infinite.

Troubleshooting
---------------
Error while installing grub:
```
source_dir doesn't exist. Please specify --target or --directory
```
Find grub2 directory in /usr/lib and specify
```
grub-install --directory=/usr/lib/grub/i386-pc --boot-directory=/mnt/boot /dev/sdb
```

