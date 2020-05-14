Rsync is protocol typically used to synchronize files and directories between for example two different systems
http://pclosmag.com/html/Issues/200712/page05.html
 or for backup.
http://pclosmag.com/html/Issues/200911/page23.html
http://www.howtogeek.com/135533/how-to-use-rsync-to-backup-your-data-on-linux/

For use rsync, you need have rsync app installed.

# **1.  How check server.**
This is website http:       **http://ftp.nluug.nl/**
This is DNS or just name, :      **ftp.nluug.nl**

DNS is name which is used instead IP number.   https://www.youtube.com/watch?v=HHK6bZeLhME


From terminal, konsole you can check, 
what folders and files it contain.

```
$ rsync -Rnv ftp.nluug.nl::
pub            	/pub hierarchy
site           	/site hierarchy
vol            	/vol hierarchy
archassault    	archassault
archlinux      	archlinux
ATmission      	AT mission Linux
centoslinux    	centoslinux
...
```

The same way you can move around on directories.
```
$ rsync -Rnv ftp.nluug.nl::pclinuxos
receiving file list ... done
drwxr-xr-x             50 2014/10/19 21:40:58 .
drwxrwxr-x          8,192 2015/08/03 20:50:13 addlocale
drwxr-xr-x             78 2016/03/08 10:25:20 pclinuxos

sent 24 bytes  received 93 bytes  78.00 bytes/sec
total size is 0  speedup is 0.00 (DRY RUN)
```

Example how synchronize srpms directory
```
$ rsync -vai4CH --safe-links --delay-updates --partial --delete-after --progress ftp.nluug.nl::pclinuxos/pclinuxos/srpms /your/target/path/

```
or
```
$ rsync -arvP --delete-after ftp.nluug.nl::pclinuxos/pclinuxos/srpms /your/target/path/
```
or
```
$ rsync -avrt --delete-after ftp.nluug.nl::pclinuxos/pclinuxos/srpms /your/target/path/
```

Options --delete / --delete-after 
will delete old files in the destination folder, which no longer exist in the source folder

**./ ** --> this is your current position in the terminal, you can it change own path,
to see in terminal where you are use **pwd** command.
```
$ pwd
/home/tele/Pulpit
```

Files first are copied to the same directory but .../~.tmp~/...
and when synchronize is finished, files are moved to correct place and you can them see.
**srpms** have 34GB and **32bit** directory  have 29GB for now.

More in **man rsync** or **rsync --help**
https://www.infoscience.co.jp/technical/rsync/original/rsyncd_conf.html

Useful links:
http://transamrit.net/docs/rsync/
http://www.jveweb.net/en/archives/2011/01/running-rsync-as-a-daemon.html
https://www.atlantic.net/community/howto/setup-rsync-daemon/


# **2. How run rsync server on UPLOS.**

**rsync** by default, it allows you to synchronize files from the rsync server.  
**(to download files)**
If you want create **rsync serwer**, you need edit config file and run rsync service. 
**(to upload files) **

- Install rsync from Synaptic
- Edit file or create new /etc/rsyncd.conf with man (manual / guide / tutorial)
```
man rsyncd.conf
```
Example rsyncd.conf,
```
[FILES]
path = /path/to/files
comment = repository

# create new specific / secure user or use nogroup
uid = nobody2
gid = nogroup
use chroot = yes
max connections = 4
read only = yes
       
#  create log
log file = /var/log/rsync.log
transfer logging = yes
log format = %t %a %m %f %b
syslog facility = local3
#  syslog facility = local5
timeout = 300

```
More in "man rsync.conf"  http://www.manpagez.com/man/5/rsyncd.conf/osx-10.4.php


**Example** 
How create restrict user account
https://unix.stackexchange.com/questions/186568/what-is-nobody-user-and-group
https://superuser.com/questions/77617/how-can-i-create-a-non-login-user
https://superuser.com/questions/830656/debian-linux-usr-bin-nologin-missing
<blockquote>When /sbin/nologin is set as the shell, if user with that shell logs in, they'll get a polite message saying 'This account is currently not available.' This message can be changed with the file /etc/nologin.txt.

/bin/false is just a binary that immediately exits, returning false</blockquote>
https://linux.die.net/man/8/useradd

```
# useradd nobody2 -r -s /sbin/nologin

#  cat /etc/passwd | grep nobody2
nobody2:x:477:412::/home/nobody2:/sbin/nologin

# ls /sbin/nologin
/sbin/nologin*

$ grep nogroup /etc/group
nogroup:x:65534:

```
If you check " cat /etc/passwd | grep nobo ", you will see "nobody" account exist, but use "/bin/sh"
so I created "nobody2"  with "/sbin/nologin".


- Tutn on rsync inside xinetd.
[u]All settings xinetd services are[/u] in **/etc/xinetd.d/**
Settings xinetd for rsync  is in /etc/xinetd.d/rsync
<blockquote># default: off
# description: The rsync server is a good addition to an ftp server, as it \
#	allows crc checksumming etc.
service rsync
{
	disable	= no
	socket_type     = stream
	wait            = no
	user            = root
	server          = /usr/bin/rsync
	server_args     = --daemon
}</blockquote>
If you set **disable	= no** , xinetd will run rsync.
If you set **disable	= yes** , xinetd will not start rsync.

About xinetd:
http://www.cyberciti.biz/faq/linux-how-do-i-configure-xinetd-service/

- Start or restart xinetd for start rsync.
```
/etc/init.d/xinetd restart
```

If you start or restart xinetd,
xinetd will run all set services from  /etc/xinetd.d/

**Info:**
```
/etc/init.d/rsync restart
```
... not will work, because this file not exist, you can try create or just use  xinetd like above.
... inside /etc/xinetd.d/ ...  you have file services, you can edit to disable or enable


Example how create repository on Debian
https://linuxconfig.org/creating-a-package-repository-on-linux-fedora-and-debian


# **3. How check running services.**
If you have rsync server, you can check how working in practice.
```
$   rsync -Rnv IP_to_serwer::
FILES            uplos rsync server
```

You can check like this with your IP internal, external and with DNS.
If working with local IP, but not working with external and DNS, check firewall and router.
If DNS not working, check DNS settings, especially IP.

This tutorial also is good.
https://www.cyberciti.biz/faq/check-running-services-in-rhel-redhat-fedora-centoslinux/

```
$ chkconfig --list
...
xinetd         	0:off	1:off	2:off	3:on	4:on	5:on	6:off	7:off

xinetd based services:
	cups-lpd:      	off
	rsync:         	on
```

If you have running rsync server from xinetd, you will also
```
# netstat -tulpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name         
tcp        0      0 0.0.0.0:873                 0.0.0.0:*                   LISTEN      6754/xinetd  
```


# **4. How debug.**
If you have problem with create working rsync server ,
- check with local IP ( 127.0.0.1 )
 " If working with local IP, but not working with external IP and DNS, check firewall and router "
Inside router you need have redirect the port from 873 to 873.
- check log file /var/log/rsync.log (path from /etc/rsyncd.conf)
- check own firewall

Example working rsync
- with local IP localhost
```
$ rsync -Rnv 127.0.0.1::
FILES         	repository
```
- with local IP
```
$ rsync -Rnv 192.168.1.1::
FILES          	repository
```

- when working
```
# netstat -anp | grep 873
tcp        0      0 0.0.0.0:873                 0.0.0.0:*                   LISTEN      5608/xinetd         
tcp        0      0 192.168.1.1:50000         192.168.1.1:873           ESTABLISHED 6000/rsync          
tcp       14      0 192.168.1.1:873           192.168.1.1:50000         ESTABLISHED 6000/rsync 
```
This show which ports are used. 

***Port 873 is used to listen connections <-- So when I want download files,  rsync will ask port  873
Port 50000 this port is used for sent files  --> so I will download files also via 50000 port***
Port  for sent files (50000) is variable, and for each connection it can use different port.
Some applications have the option of setting constant port, it helps to seal our firewall.
This know is important, because you will know which ports you need open inside firewall. 

**Example errors:**
```
rsync: failed to connect to xxx.xxx.xxx.xxx (xxx.xxx.xxx.xxx): Connection refused (111)
rsync error: error in socket IO (code 10) at clientserver.c(125) [Receiver=3.1.2]
```
- rsync or xinetd service / daemon not running or your firewall blocks access

```
@ERROR: chdir failed
rsync error: error starting client-server protocol (code 5) at main.c(1648) [Receiver=3.1.2]
```
- folder with files not mounted or not exist.


# **5. Restrict rsync access using IPtable Firewall Rules**
```
ETH="eth0"
YOUR_IP= "178.36.xxx.xxx"
iptables -A INPUT -i $ETH -p tcp -m tcp -s $YOUR_IP --dport 873 -m state --state NEW,ESTABLISHED -j ACCEPT -m comment --comment "server_rsync"
iptables -A OUTPUT -o $ETH -p tcp --sport 873 -m state --state ESTABLISHED -j ACCEPT -m comment --comment "server_rsync"
```
In this case,  restriction this is
-  used interface ( **eth0** - allows you send only from this interface ) 
-  IP ( **178.36.xxx.xxx** - allows you send only to a person with this IP ) 
You can also restrict to accept incoming connections from specific MAC address. ( -m mac --mac-source 00:mac:address:8b )
Sometimes you can also restrict ports for server response, if your server use a small range of ports.
You can also change the default port 873 ( for app and firewall ), if is possible.
 ( changing the port is not a security measure, but only hinders the work of simple scanners, if the server will available for everyone )

Other examples with iptables https://linuxconfig.org/collection-of-basic-linux-firewall-iptables-rules

Check available services  / open ports with **nmap** online and with own **nmap** to see how working your firewall / check if the rsync service is available.
```
nmap Your_IP
```

**Example for Gufw Firewall**
Serwer IP= "192.168.a.a" port "873"
Client IP= "192.168.b.b"  port variable
<blockquote>**OUTPUT**
From: 192.168.a.a  port: 873
To: 192.168.b.b
**INPUT**
From: 192.168.b.b
To: 192.168.a.a  port: 873</blockquote>
You can block the connection input  in and output and rsync with this rule will still work.


***Edited 4.01.2019***
- Added about option --delete
