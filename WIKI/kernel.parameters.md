Kernel allows you, run the system with various options
Example list https://www.kernel.org/doc/html/v4.14/admin-guide/kernel-parameters.html

How add options for:
__________________________________{
# **GRUB Legacy**

Open **/boot/grub/menu.lst**
add option to line
```
kernel (hd0,0)/boot/vmlinuz BOOT_IMAGE=linux root=LABEL=UPLOS  nokmsboot splash quiet vga=788
```

For example I want add option **ipv6.disable=1**
and part of file it will looks like this

```
...

title linux
kernel (hd0,0)/boot/vmlinuz BOOT_IMAGE=linux root=LABEL=UPLOS  nokmsboot splash quiet vga=788 ipv6.disable=1
root (hd0,0)
initrd /boot/initrd.img

...
```
That's all. Restart the computer and check :)
__________________________________}


__________________________________{
# **GRUB2**

To add kernel options / parameters, open **/etc/default/grub**
and add option to line
```
GRUB_CMDLINE_LINUX_DEFAULT="first_option second_option"
```

GRUB2 after this, need update from terminal
```
update-grub
```
and reboot computer.

Exist also **grub-customizer** app in the repository.
For people who like a GUI for edit GRUB.
Probably it  only works with grub2.
__________________________________}


More about GRUB Legacy in
https://www.gnu.org/software/grub/grub-legacy.html


# The most used options:

**# 1. libata.noacpi**

```
libata.noacpi=1
```
This fix some problems with older or incompatible Bios / UEFI 
http://blog.le-vert.net/?p=24

To show ACPI errors, you can check from terminal 
```
dmesg | grep ACPI
```
and search errors.

**Eliminating errors can speed up the start of your computer.**
 :D

Very often ACPI, is also off.
However, disabled acpi is not recommended,
 because ACPI is to communicate with the hardware.
Especially in laptops ( to turn off,  to change the brightness, to change the volume ).
https://askubuntu.com/questions/139157/booting-ubuntu-with-acpi-off-grub-parameter


**# 2. ipv6.disable**

```
ipv6.disable=1
```
Is used for disable IP v6. 
When you have a new kernel, it supports and uses IP v4 and IP v6
You can check it with command
```
ifconfig
```
IPv4 = inet 
https://ozmoroz.com/images/2012-10-24-oralinux3.png
IPv6 = inet6
https://farm8.staticflickr.com/7282/16415082398_5fb0920506_b.jpg

More in http://ask.xmodulo.com/disable-ipv6-linux.html
**
If you don't use IPv6, it is recommended to switch off.**
Because,
- you must remember about strengthening the firewall ( for IPv6 )
- if you use IPv4, and IPv6 working, IPv6 can be used to hack / break IPv4 
and consequently to break into a computer.

**It is a good practice to turn off everything what is unnecessary.**
:D


**# 3. nomodeset**
```
nomodeset
```
Many new linux users, after installing Linux distribution, greets the black screen, with a flashing cursor.
It applies sometimes older and newer devices.
<blockquote>The first troubleshooting step that may resolve your boot issue  ...
is to disable the intel graphics features by setting the "nomodeset" option before boot.</blockquote>

A very good guide with pictures
http://www.dell.com/support/article/pl/pl/pldhs1/sln306327/manual-nomodeset-kernal-boot-line-option-for-linux-booting?lang=en

**# 4. Another used often.**
```
quiet
```
" quiet           [KNL] Disable most log messages"
```
splash
```
I can not find info about this option, maybe this help a little
https://askubuntu.com/questions/716957/what-do-the-nomodeset-quiet-and-splash-kernel-parameters-mean/716973


***Edited***
5.03.2018
PS: Please do not spam on the wiki, 
**if you do not want** 
to ask, edit, add info / knowledge.


Edited:
- Added red color.
