###############################################################################################################


########################################
# **MyliveCD - Remastering, create own ISO **
########################################

## **Edit own ISO, Remastering**

Before create own ISO you should know.
**MyLiveCD** or **MyLiveGTK** build ISO from system which was started.
It will create a copy of the whole system which you are using,
REMEMBER about it and do not leave personal property if you build ISO for other people.

For example if you build new public ISO, you can 
- use fresh, updated system installed on virtualbox / computer
- change users and password ( root - root ,  guest -guest )
- run auto login
- after build ISO **create checksum** for people who want verify downloaded ISO
https://en.wikipedia.org/wiki/Md5sum
http://www.liberainformatica.it/forum/showthread.php?tid=719

You can add "Hello world !" , guide, install or remove packages from system, ...
Settings, folders for new user you can change in **/etc/skel/**
- Please remove from this folder old, unused settings.
- More info about  **/etc/skel/** you can find in web search.


***Tips:***
1.) How change default wallpaper for Mate
Fast way , try change--> /usr/share/backgrounds/mate/desktop/Stripes.png
or
try change  /usr/share/glib-2.0/schemas/org.mate.background.gschema.xml
then run from root in terminal
```
glib-compile-schemas /usr/share/glib-2.0/schemas/
```

2.) For help find unnecessary packages you can use
- **rpmorphan** ,
but be careful before remove packages, because sometimes package may be important.
For example:
Command rpmorphan show libiptables5,
and inside Synaptic ( package manager ) you see
*libiptables5  2013
libiptables10 2016*
then you see libiptables5 is old and when you trying remove this package don't have dependencies,
so you can remove libiptables5. 
-  **rpmreaper**
https://www.pclinuxos.com/forum/index.php/topic,132864.msg1131438.html#msg1131438
man https://linux.die.net/man/1/rpmreaper

3.) Tips for GRUB
https://wiki.archlinux.org/index.php/GRUB/Tips_and_tricks

4.) Security
 You can check
- file permissions
- set firewall
- check listen apps
```
netstat -ntulp
```
- check processes, search zombies
 ( some damaged applications can be deleted, for example if are broken : s2u, xdg-user-dirs, xdg-user-dirs-gtk )


## **Build own ISO**

We suggest use this scheme ISO name :

For UPLOS developers:
**UPLOS-desktop_environment-date-language.iso**

For community, if you are not a UPLOS developer 
**community-UPLOS-desktop_environment-date-language.iso**

- language should contain two-letter codes from ISO 639-1
https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
- language not must be included if it is English language by default
- to the name of desktop environment can be added additional name, reconnaissance name your ISO
for example:
**UPLOS-trinity-2017.01-en.iso
community-UPLOS-trinity-2017.01-en.iso**

***Or do not use the name UPLOS inside ISO name,
to avoid confusion and misinterpretation with the original name and version of UPLOS Linux distribution.***

**1. Build ISO With MyLiveCD in terminal.**
 To create your own livecd, install draklive-install (system installer):
( You can install from Synaptic if you like )
```
apt-get install draklive-install
```

and mylivecd:
```
apt-get install mylivecd
```

then 
```
mylivecd --nodir=^/tmp --md5sum --verbose name.iso
```

After "--nodir=^ " are folders which will not be taken to ISO
Others paths which you can exclude:  ( you need test which you can use )
```
^/[.].*,^/fastboot$,/core[.][0-9][0-9]*$,.*~$,.*[.]rpmnew$,.*[.]rpmsave$,[.]bash_history$,[.]fonts[.]cache-[0-9]$,[.]xauth.*,[.]wh[.]*,[.]xsession-errors$,^/var/run/,^/var/lock/subsys/,^/var/lib/dhcp/,^/root/.kde4/share/config/kmix*,^/root/.kde4/share/config/phonondevicesrc,^/home/guest/.kde4/share/config/kmix*,^/home/guest/.kde4/share/config/phonondevicesrc,^/etc/modprobe.conf,^/etc/sysconfig/network-scripts/ifcfg-eth*,^/etc/udev/rules.d/70-persistent*,^/etc/asound.state,^/etc/X11/xorg.conf,^/etc/lilo.conf,^/boot/grub/menu.lst,^/etc/modprobe.preload.d/cpufreq,^/etc/sysconfig/harddrake2/previous_hw,^/var/lib/rpm/__db.*,^/etc/sysconfig/finish-install,^/var/lib/speedboot/*,^/etc/modprobe.d/sound,^/var/log/mysqld/mysqlmanager.log
```
***- And remember to unmount other partitions and disks or use ^/media/***

Example script for own use, for build ISO with system date:
```
#!/bin/bash
DATE=`date +%Y-%m-%d`
mylivecd --nodir=^/tmp,^~/Desktop/TEST --md5sum --verbose uplos-mate-${DATE}.iso
```

**2. Build ISO With MyLiveGTK**
MyLiveGTK this is graphical user interface for mylivecd.
Install **mylivegtk** and enjoy available options.

***Before build own ISO, remember prepare a little more space in system or hard drive.
I do not remember exactly , but  ...
this mean, if your system use 5GB on hard drive, you will have about 2,5GB ISO .***


**3.Build ISO with chroot / mksquashfs  - only for advanced users.**
https://wiki.mageia.org/en/Remaster_Mageia_Live_Media_Selfmade_ISO


###############################################################################################################

__________________________________________________________________________________________________________________________________________________________________________________

###########################
# ** Create LiveUSB**
###########################
LiveUSB you can create in 2 ways:

**1. Use any software for create bootable USB**,
for example

- Unetbootin ( Warning: some versions may not work with UPLOS )
https://unetbootin.github.io/

- MultiSystem ( working very well, but from own live-cd, for boot live-cd you can use virtualbox )
http://liveusb.info/dotclear/

- YUMI  ( on Windows only )
- Rufus  ( on Windows only )
- Other :  http://alternativeto.net/software/unetbootin/

Info: 
You can try run windows apps with "wine", but not all must working.
https://askubuntu.com/questions/314205/how-to-mount-usb-flash-drive-to-wine/352929#352929
And files should be move below or to ~/.wine  directory.


**2. With dd command in linux terminal.**
But before use dd command ISO should be hybrid,
only this way iso can boot from USB.

A). How check ISO, if is hybrid or not ?
Example:
Not hybrid ISO
```
$ fdisk -l manjaro-mate-15.12-x86_64.iso
Disk manjaro-mate-15.12-x86_64.iso: 7,5 GiB, 8006074368 bytes, 15636864 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000
```

Hybrid ISO
```
$ fdisk -l manjaro-xfce-16.08-x86_64.iso 
Disk manjaro-xfce-16.08-x86_64.iso: 1,5 GiB, 1561657344 bytes, 3050112 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x06c2dccb
.
Device                         Boot Start     End Sectors  Size Id Type
manjaro-xfce-16.08-x86_64.iso1 *        0 3050111 3050112  1,5G  0 Empty
manjaro-xfce-16.08-x86_64.iso2        224   63711   63488   31M ef EFI (FAT-12/16/32)
```
Isohybrid have 2 partitions.
You can check this also with gparted after burn the iso on an usb stick. 

B). How create isohybrid ISO
```
isohybrid -v /path/to/name.iso
```
or for UEFI support
```
isohybrid --uefi -v output.iso
```
uefi option may be important, because sometimes USB may not want boot with or without this option.

**Warning:***
      Not all ISO are isohybrid ! However you can check and create own isohybrid ISO. 
    If you use very old hardware and rare, may not support isohybrid ISO. 
    Isohybrid created for UEFI should work also for "Legacy mode" Bios, without support UEFI not will work on UEFI. 
    If you use "Legacy mode" Bios and isohybrid UEFI ISO not working for you, try create or rebuild ISO with " isohybrid " command, but without  " --uefi " option. 
    Before use USB stick check Bios/UEFI settings, USB should start first.  *

More in http://www.syslinux.org/wiki/index.php?title=Isohybrid

C). Writing ISO to a USB Stick in Linux with dd command
```
dd bs=4M if=/path/to/name.iso of=/dev/sd[drive letter] status=progress
```
Where [drive letter] is the letter of your removable device. Please note that it is the device (e.g. /dev/sdb), and not the partition number (e.g. /dev/sdb1).

To find which drive letter it might be write from root: 
```
fdisk -l
```

Execute the command, insert the pendrive, reissue the command, and see which new line appeared. 
 Usually it will /dev/sdc, but not always. And here from " /dev/sdc " , " c " this is drive letter.

Instead " fdisk -l " you can use 
```
lsblk
```
or
```
blkid
```


Example script to detect usb pendrive:
```
#!/bin/bash

read -rsp $'Please remove usb pendrive and press Enter. \n' -n1 key ; \
a=$(lsblk) ; read -rsp $'Insert usb pendrive, and Wait 5 seconds then press Enter.\n' -n1 key ; \
b=$(lsblk); diff <(echo "$a") <(echo "$b") 
```

How working ?
I copy the script to a file " usb.test " , I give the file the right to execute, and I run script from terminal, follow the instructions.
```
$ ./usb.test
Please remove usb pendrive and press Enter. 
Insert usb pendrive, and Wait 5 seconds then press Enter.
23a24,25
> sdc                            8:32   1   7,5G  0 disk 
> └─sdc1                         8:33   1   647M  0 part 

```
And here " sdc " this is name what I looking. :-)
So command for me will looks like this " dd bs=4M if=/home/tele/Download/uplos.iso of=/dev/sdc status=progress "

Warning:
Option " status=progress " for example on Debian 9.0 may not work.
Instead above command you can try install **pv** and use
```
pv /path/to/uplos.iso | dd of=/dev/sd[your drive letter for usb device] bs=4M
```
In my test, pv command gives only partial satisfaction, because when the command is terminated,
the pendrive ( USB flash drive ) still works for a while.

Of course,
instead of this script for detect usb, you can copy last 3 lines of the script and paste to terminal and such a long command should also work,
or just you can use " lsblk " or other command by hand in terminal to detect device.

More about device names in Linux
https://www.debian.org/releases/stable/mips/apcs04.html.en
https://en.wikipedia.org/wiki/Device_file#Naming_conventions


**USB stick issues.**
1. When after the interruption of the write operation or after created wrong partition table clean not working.
- Try restart your computer for clean kernel cache, then you can try clean USB stick.

Example command for clean USB stick
```
dd if=/dev/zero of=/dev/sd[drive letter]
```
More in https://en.wikipedia.org/wiki/Dd_(Unix)


Partition tools:
fdisk guide   http://www.redips.net/linux/create-fat32-usb-drive/
parted manual   https://www.gnu.org/software/parted/manual/parted.html
gparted manual  http://gparted.org/display-doc.php?name=help-manual


*Edited: 11.11.2017*
- added about rpmreaper
*Edited: 19.09.2017*
*- added about security*
