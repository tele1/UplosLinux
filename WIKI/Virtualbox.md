**# 1. About Networking Modes.**



# **Networking modes.**
https://www.virtualbox.org/manual/ch06.html#networkingmodes
Check "Table 6.1. Overview" inside link.

**How to use them ?**
For example I have one virtual machine on real system.
And inside "Settings" virtualbox for virtual machine, default is used **NAT**
__example:  https://dbaportal.eu/2017/03/15/using-ssh-tunneling-to-gain-access-to-remote-virtualbox-guest-attached-to-nat/  
NAT is cool because connects to the internet and you can redirect ports in advanced settings virtualbox.

Check own IP inside virtual machine with **ifconfig**  http://k2schools.com/how-to-find-ip-address-on-linux/
Check own external IP https://www.whatismyip.com/

If you want synchronize files via rsync from virtual machine to real system,
try change inside virtualbox settings network attached to **Bridged networking**, instead NAT.
What will change ?
- Inside virtual machine IP will looks like "192.168.1.x"
This IP (192.168.1.x) you can use inside real system to synchronization files from virtual machine. 
This is faster and safer, because you do not need synchronize files via the external internet.
However if the data must be confidential, disconnect the internet, configure the firewall and port forwarding
 or configure the firewall, port forwarding and use ssh instead rsync.

Info: 
If your virtual machine running, and you change the settings inside virtualbox
then restart virtual machine for making changes.


**Example with rsync.**
**Computer_1** [ IP=**192.168.1.7** ] --> router [ IP=192.168.4.500 ]  --> **Computer_2** [ IP=192.168.1.8 ] ( Virtual_Machine_2.1 )

If rsync server running on **Computer_1** , 
on **Virtual_Machine_2.1** I can check files from **Computer_1**
<blockquote>rsync -Rnv **192.168.1.7**::FILES/
receiving file list ... done
drwxr-xr-x          4,096 2018/10/27 13:42:48 Guides

sent 20 bytes  received 101 bytes  242.00 bytes/sec
total size is 0  speedup is 0.00 (DRY RUN)
</blockquote>

#====================================

**# How update virtualbox manually from terminal**

*Info: *
**$** --> user account
**#** --> root accout

**Warning**
- Virtualbox has its own installer over which we do not have much control - VirtualBox-5.2.22-126460-Linux_x86.run
I mean that you should know where files are installed and what permissions they have.
However this installer have more options than only "install".
- After install I don't see dkms.conf for virtualbox, this means that you will have to manually compile drivers for the new kernel.

#=================================

**1. Download newer versions Virtualbox**
- Today's latest it is **VirtualBox Version 5.2.22**

curl -O  --> O like Odin ( not zero )
```
$ curl -O https://download.virtualbox.org/virtualbox/5.2.22/VirtualBox-5.2.22-126460-Linux_x86.run
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 83.7M  100 83.7M    0     0  2894k      0  0:00:29  0:00:29 --:--:-- 2996k
```


**2. Verify**

<blockquote>$ curl -v --silent https://download.virtualbox.org/virtualbox/5.2.22/SHA256SUMS --stderr - | grep VirtualBox-5.2.22-126460-Linux_x86.run | sha256sum --check
VirtualBox-5.2.22-126460-Linux_x86.run:** OK**</blockquote>
Do you see "**OK**" ? If yes, file is good.



**3. Uninstall older versions**

```
# bash ./VirtualBox-5.2.22-126460-Linux_x86.run uninstall
Verifying archive integrity... All good.
Uncompressing VirtualBox for Linux installation.............
VirtualBox Version 5.2.22 r126460 (2018-11-08T20:19:39Z) installer
VirtualBox 5.2.22 r126460 has been removed successfully.
```


**4. Uninstall older VirtualBox drivers.**

Check dkms 
```
# dkms status
vboxhost, 5.0.6, 3.14.7-pclos1, i586: installed 
```

```
# dkms remove -m vboxhost -v 5.0.6 --all
```

You can also remove files, if exists
```
# rm -vfr /var/lib/dkms/vboxhost
# rm /usr/src/vboxhost-5.0.6
```

Ckeck also
<blockquote># modinfo vboxdrv
filename:      ** /lib/modules/4.1.13-pclos1/kernel/misc/vboxdrv.ko**
version:        5.0.20 r106931 (0x00240000)
license:        GPL
description:    Oracle VM VirtualBox Support Driver
author:         Oracle Corporation
srcversion:     w0i0
depends:        
vermagic:       4.1.13-pclos1 SMP mod_unload 686 
parm:           force_async_tsc:force the asynchronous TSC mode (int)</blockquote>

If you see driver, remove him
<blockquote># rm **/lib/modules/4.1.13-pclos1/kernel/misc/vboxdrv.ko**
rm: remove regular file ‘/lib/modules/4.1.13-pclos1/kernel/misc/vboxdrv.ko’? y</blockquote>


**5. Install new Virtualbox.**

```
# bash ./VirtualBox-5.2.22-126460-Linux_x86.run
```


**6. Build new drivers.**

```
# /sbin/vboxconfig
vboxdrv.sh: Stopping VirtualBox services.
vboxdrv.sh: Starting VirtualBox services.
vboxdrv.sh: Building VirtualBox kernel modules.
vboxdrv.sh: Starting VirtualBox services.
```

You should see new driver
<blockquote># modinfo vboxdrv
filename:       /lib/modules/4.1.13-pclos1/misc/vboxdrv.ko
version:        **5.2.22** r126460 (0x00290001)
license:        GPL
description:    Oracle VM VirtualBox Support Driver
author:         Oracle Corporation
srcversion:     0nA
depends:        
vermagic:       4.1.13-pclos1 SMP mod_unload 686 
parm:           force_async_tsc:force the asynchronous TSC mode (int)</blockquote>

And VirtualBox should work.


**7. Check linux file permissions.**

For example like this
```
# find / -perm -o+w -type f  -not -path "/proc/*"
find: ‘/home/tele/.gvfs’: Permission denied
```
This command will
- search writable files
- search only files for exclude symbolic links
- exclude path /proc/
