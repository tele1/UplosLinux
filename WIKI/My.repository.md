I have reinstalled the system and thought I should write a bit soon, because later, it will be harder.
Topic will grow with time.


Why I don't want a real server ?
- Repository for indexing packages needs apt-rpm, so needs installed Uplos or PCLinuxOS
- Installations of dedicated systems provide only dedicated servers / hardware and they are expensive.
Today on the internet is a lot of clouds for sale, but there you can only install adapted the system or choose one of several.
- Clouds or dedicated servers do not guarantee sufficient security in my opinion.
Especially since we have to check security ourselves.
* For me, password protection is not enough.
* Checking the security from the console if it exists.
* Checking if there are other default accounts and passwords.
* Some IP addresses can often be scanned, by bots.
However, everyone can assess the safety alone, because you can create a checksum of files and copies files for security.
- Providing security without ready-made guides and tools can be difficult.
Some tools are configured a little differently, and it takes some time.


Why on virtualbox ?
I wanted to have several machines for different tasks.
So, I could choose **virt-manager** or **virtualbox** 
( qemu not, because you need can configure, because it can be slow, however can be used also from virt-manager and maybe working better )

On the real system.
- Automatic system shutdown at power failure
In the future should also disable the system during an attack from the outside.
( A secure computer is a computer that is turned off :) )
- Internet firewall typical for the server with iptables.
- You can also secure files in the system, according to the guides on the web

On a virtual system / machine.
- Internet firewall typical for the server with iptables and sometimes with open port for rsync.
- Rsync server
- Script for updating indexes of files
- Other scripts for check packages

======================================

Transferring Files
When you want to send files directly to someone, you have to give him, your own IP address.
instead of an IP address, you can give an address www., called  DNS or DDNS or FQDN
However more correct name should be FQDN, in my opinion.
I have dynamic IP, so I use DNS.
DNS address you can get it for free or buy it.
IP changes almost every day, so something or someone has to assign IP to DNS.
- you can do it manually 
- you can install the program
- some routers have options DNS, there you can choose or add a company which gives DNS.
The main disadvantage of this, if the program or router breaks down or if you will too slow, then the DNS address will redirect to old / invalid IP address.

How DNS look like in practice?
For example if I check Rsync with IP
```
rsync -Rnv 77.7.7.xx::
```
with DNS loks like this
```
rsync -Rnv uplos.webname.en::
```

Probably the easiest example script to check and update DNS. (of course, change http to https)
https://freedns.afraid.org/scripts/update.sh.txt
And in script you should add, save status output to log. 
And this script should be running from "cron".

From where get DNS ( try search DDNS )
examples:
https://www.keycdn.com/blog/best-free-dns-hosting-providers
https://www.maketecheasier.com/best-dynamic-dns-providers/
https://www.gnutomorrow.com/best-free-dynamic-dns-services/
https://www.ionos.co.uk/digitalguide/server/tools/free-dynamic-dns-providers-an-overview/
https://now-dns.com/

Info: You can find also DNS for web browser, this is something other.


Security DNS
Refreshing IP for DNS sometimes is very simple
It use api key http://freedns.afraid.org/scripts/update.sh.txt
It use password https://dns.he.net/
I don't know if exist block / ban IP for person which wants to guess api key or password. You have to check yourself.
Is the password via https better than api key ?
- if the password does not have too much restriction then probably yes.


Is https always safe?
- not.    
https://stackoverflow.com/questions/26671599/are-security-concerns-sending-a-password-using-a-get-request-over-https-valid
https://blog.httpwatch.com/2009/02/20/how-secure-are-query-strings-over-https/

How check?
Run in first terminal.
```
tcpdump -X  > log.txt
```
Login with app or command. 
Close all apps and if you can find in log.txt file unencrypted data, login, password then your connection is not encrypted.


======================================

About IP
IPv4 and IPv6 are incompatible.  https://en.wikipedia.org/wiki/IPv6
This means that your browser will see websites only from IP v4 or only from IP v6.
Most people in the world has a dynamic IP v4, constant IP v4 is payable.
Constant IP v6 is for free (for now), but it is not widely used.

Packages removed
s2u, xdg-user-dirs, xdg-user-dirs-gtk, numlock, setxkbmap (Zombie process)

For disable IPv6 added ipv6.disable=1 
to /boot/grub/menu.lst
```
...
title linux
kernel (hd0,0)/boot/vmlinuz BOOT_IMAGE=linux root=UUID=<number>  splash quiet noiswmd vga=788 ipv6.disable=1
root (hd0,0)
initrd /boot/initrd.img
...
```

From "Control Center" --> "Startup Applications"
disabled:
- Mate Login Sound (because sound carrd I have disabled)
- Certificate and Key Storage
- Power Manager
- PulseAudio Sound System
- Qt-update-notifier-autostart
- Screensaver
- Secret Storage Service
- SSH Key Agent
- User folders update
- Volume Control
( If something not will work, I can enable again )


Remove not needed process
"UPLOS Control Center" --> "System"
Disable not needed services and daemons.


Rsync configured
http://www.liberainformatica.it/forum/showthread.php?tid=331


Import Public Keys For RPM
```
# rpm --import *
```
http://www.liberainformatica.it/forum/showthread.php?tid=1267


Intrusion Detection
Read about AIDE  http://www.liberainformatica.it/forum/showthread.php?tid=1031


Read about Lynis  , Tiger  , Logwatch .


Read about  " Linux Server Hardening Security Tips "
https://www.cyberciti.biz/tips/linux-security.html


Best light file managers
EmelFM2  https://en.wikipedia.org/wiki/EmelFM2
Tux Commander  https://ubuntuforums.org/showthread.php?t=50965
Ideal for working with large amounts of files.


Script for refresh local rpm repository. ( script to create database about packages )
```
# ls -l /usr/bin/updaterep
-rwxr----- 1 root root 1157 Nov 15 23:44 /usr/bin/updaterep*
```

```
#!/bin/bash

if [[ "$EUID" -ne "0" ]] ; then
     echo " Warning: This script must be run as root! "
     echo " Try again as root, the end. "
		exit 1
fi

echo "                       "
# This will fix permissions after file transfer from cloud storage.
echo "--> Fix permissions ..."
chown -R root:users /path/to/your/REPOSITORY
find /path/to/your/REPOSITORY/ -type d -print0 | xargs -0 chmod 0755
find /path/to/your/REPOSITORY/ -type f -print0 | xargs -0 chmod 0644
# Other way #  find /your/location -type f -exec chmod 644 {} \;

echo "                       "
echo " --> Update rpm ..."
apt-get clean;
genbasedir --flat --bloat --progress /path/to/your/REPOSITORY/32bit cinnamon-test devel kde kde4 kde4-test kde5 kde5-test mate mate-test nonfree updates xfce4 xfce4-test;
apt-get update
    
echo "                       "
echo " --> Update src.rpm ..."
genbasedir --flat --bloat --progress /path/to/your/REPOSITORY/srpms uplos
```


About tools
https://blog.serverdensity.com/80-linux-monitoring-tools-know/


Several commands from notepad. ( commands mixed with systemd )

SENT VIA SCP / SSH
```
# Download
scp -P 57185 root@212.59.240.XX:/backup.tar.gz /home/tele
======
# Upload
scp -P 57185 /home/tele/ISO/uplos-mate-2017-03-15-en.iso root@212.59.240.XX:/home/vnc/virtualbox/
```

VIRTUALBOX
```
VBoxManage startvm $VM --type headless
VBoxManage list vms
VBoxManage list runningvms
VBoxManage controlvm $VM poweroff
```

INSTALL LOCATE COMMAND
```
apt-get install mlocate
updatedb
```

VNC
```
- Install
apt-get install tightvncserver
=============
- Start server
su name_user
vncserver
=============
- Connect with server
vncviewer 46.242.130.XX:5901
=============
- Kill server from name_user
vncserver -kill :1
=============
sshd -T | grep -i port
```

VNC TUNNELED VIA SSH
https://tecnstuff.net/how-to-install-and-configure-vnc-on-debian-9/
```
ssh -L 5901:127.0.0.1:5901 -N -f -l  root 46.242.130.XX
vncviewer 127.0.0.1:5901
```

REMOVING UNNECESSARY SERVERS
```
# netstat -ntulp
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      9997/sshd       
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      555/rpcbind 
=============================
CHECK:
# ps aux | grep rpcbind 
root       555  0.0  0.0  37080  2652 ?        Ss   Apr23   0:00 /sbin/rpcbind -w
# dpkg -S /sbin/rpcbind
rpcbind: /sbin/rpcbind
=============================
REMOVE:
apt-get remove  rpcbind
OR WITH CONFIG FILES
apt-get --purge remove nazwa
CHECK
dpkg -l | grep -E ^rc 
```

REMOVING UNNECESSARY SERVERS (example for systemd)
https://dynacont.net/documentation/linux/Useful_SystemD_commands/
```
# netstat -tulpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      572/sshd            
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      974/exim4           
tcp6       0      0 :::22                   :::*                    LISTEN      572/sshd            
tcp6       0      0 ::1:25                  :::*                    LISTEN      974/exim4           
udp        0      0 0.0.0.0:37281           0.0.0.0:*                           291/avahi-daemon:
==========================
# systemctl
exim4.service                                       loaded active running   LSB: exim Mail Transport 
ssh.service                                         loaded active running   OpenBSD Secure Shell serv
anacron.timer                                       loaded active waiting   Trigger anacron every 
avahi-daemon.service                                loaded active running   Avahi mDNS/DNS-SD Stack

```
```
# systemctl disable  ssh.service
Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install disable ssh
Removed /etc/systemd/system/sshd.service.
===============================
# systemctl status  ssh.service
● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; disabled; vendor preset: enabled)
   Active: active (running) since Thu 2019-07-04 13:26:57 CEST; 23min ago
...
===============================
# systemctl stop  ssh.service
===============================
# systemctl status  ssh.service
● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
...
```
:D  https://en.wikipedia.org/wiki/Anacron 
<blockquote>Stable release	
2.3 / July 16, 2009; 9 years ago</blockquote>




STOP BROKEN MODULS
```

# systemctl list-units | grep -iE 'failed|not-found'
● atd.service                                                                              loaded    failed failed    Deferred execution scheduler
● mdmonitor.service                                                                        loaded    failed failed    MD array monitor
● rpcbind.service                                                                          not-found active exited    rpcbind.service
================
systemctl reset-failed
systemctl disable rpcbind.service
systemctl stop rpcbind.service 
```

FIND IP AND PORT FOR FIREWALL
( This commands will show IP only when you allow to connection )
```
# lsof -i -P
megasync  15751   tele   78u  IPv4 887630      0t0  TCP 192.168.XXX.XXX:37160->185.206.XXX.XXX:443 (ESTABLISHED)
=================
# netstat -apn | grep megasync
tcp        0      0 192.168.XXX.XXX:33658       89.44.XXX.XXX:443       ESTABLISHED 15751/megasync
=================
PACKET ANALYZER:
# tcpdump  
```


Protection of services by firewalls.
Firewall gives limited protection. 
For example if you need protect SSH read about Fail2Ban, Port knocking, Single Packet Authorization.
You can also use DDNS and fingerprint OS to limit the number of IP addresses to your computter.

Linux - Resource Manager 
http://www.liberainformatica.it/forum/showthread.php?tid=1280&highlight=limit

*Updated -31.07.2019 -  about DDNS  and protection of services*
*Updated -09.09.2019 -  adeded link to Resource Manager *

Rest another time.
