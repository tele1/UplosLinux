# 
**YOU BE AWARE**

        Everything you have on the devices, maybe one day be on the internet. 
        So, do not put things which you do not want on the internet 

        If you're scared, also keep positive thoughts
        fear is good because it warns you, but do not get paralyzed.
        We should act wisely, in order to cope in life.


**FIREWALL**

*     Read about firewalls
*     Check firewall, because it is the basis of every system.
          ```
iptables -L
```
*     Read about IPv4 and IPv6, how check which you use, how disable not used
*     Read how read firewall logs
*     Read how find open ports. Check active TCP / UDP connections.
         ```
netstat -ntulp
```
         http://www.liberainformatica.it/forum/showthread.php?tid=949
<blockquote>Do services that connect to the internet should use root?
- root permissions are not always required. For example is not recommended for ssh.

```
# netstat -nltpe
Active Internet connections (only servers)
...            State       User       Inode      PID/Program name  
...            LISTEN      0          8645       3593/cupsd

# id root
uid=0(root) gid=0(root) groups=0(root)
```
If you remove the option "n", you will see the name instead of letters
All groups are in /etc/group </blockquote>
*     Read how find applications that use the internet and how to block
*     Read how listen to the connections / applications. Secure passwords should be encrypted
*     Read how block dangerous websites and access to the router, to protect the system and router against attack from web browser.


**WEB BROWSER**

*    Read what data is disclosed by the web browser
*    Read about plugins: **uBlock Origin** , **NoScript**
*    Read why installing unknown or poor plugins can be dangerous.
        Disable or remove not needed plugins ( for example: old flash player )
*    If you share files through links, always delete old not used links.
        Most links, from " cloud storage " have a random link
        and when you delete file, other person can get your link.
*    Clean often your data and stories in the browser.
        - browser will be faster
        - dangerous scripts will deleted
*    Read about **displaying punycode** ( Phishing Attack Uses Domains Looks Like Identical to Known Safe Sites )
        http://www.liberainformatica.it/forum/showthread.php?tid=772&highlight=punycode



**E_MAIL**

*    Read how to create aliases for your own mail and why can help you protect against spam and how inform about a data leak
*    Read how how to automatically sort trusted emails
*    Read how read source code messages and how to see the headers
*    Read about phishing and punycode phishing attack


**SYSTEM PROCESSES**

*    Read about sandbox
*    Read how prevent a fork bomb by limiting user process


**GOOD CUSTOMS**

*    Do not run commands, if you do not understand what they are doing.
<blockquote>- you can use the internet search engine, for check command
- if the guide is weak, look for another one too
- for help you can use also "command --help" , "man command" , https://explainshell.com/ </blockquote>
*    Do not rewrite too long commands in terminal.
         Better way is use "copy" and "paste".
*    Do not use root account if you don't need.
*    Do not trust anyone. If you can check, verify. Be careful, be preventive.
<blockquote>*Friend stories:
She thought he was writing with a friend ( she / her ).
and she gave her own password.
The truth was different:
the friend's account has been hacked and she unknowingly gave the password to the hacker.
( Although she could call to verify whether he writes. )

From the radio:
Hacker introduced himself as an employee of the bank and asked for login and password.
Some people get fooled and money was stolen.*
 </blockquote>

*    Do not click on the links if you do not have to.
<blockquote>        Links are often hidden, like this [url=https://www.google.com]www.not_safe.ccom[/url]
        Click the right mouse button and select "** Copy the address of the link **" and paste somewhere, you will see real link "https://www.google.com"

        **Do not trust also google links from friends.**
        because not every link is from a search engine. 

        Example links:

*        https://www.google.com/url?sa=
            This is the beginning of a shortened link ( redirecting to another page).
            Shortened because:
            - it may redirect to hidden the real hacker webside 
            - or it may just only redirect to a very long link
            - redirection can also be used to collect data through other websites
*        https://www.google.pl/search?q=
            This is the part of the real search engine link
  </blockquote>
*    Read how to build strong passwords
        Password should be long and consisting of various characters.
        You can encrypt words, it can protect against dictionary attack. Try create your own cipher.
*    Read about encryption / ciphers on wiki and other websites. For example about
- Transposition cipher
- Substitution cipher
- Polybius square
*    Change the password sometimes, where protection is most important.
*    Do not leave visible passwords.
        You can use program to save and encryption passwords or save to paper notepad.
*    Do not use the same passwords, on different websites, ...
*    Read about two-factor authentication.
        You can useit  for login. This is additional security, if you need to take care of data security.
<blockquote>Examples:
- For github
https://help.github.com/articles/configuring-two-factor-authentication/
- For SourceForge
https://sourceforge.net/p/forge/documentation/Multifactor%20Authentication/    </blockquote>
*    Read about GPG / GnuPG ( Asymmetric encryption with 2 keys: private and public )
*    Read about hash collisions
        https://en.wikipedia.org/wiki/Collision_attack
        http://valerieaurora.org/hash.html 
*    Read about Honeypots
        https://en.wikipedia.org/wiki/Honeypot_(computing)
        http://www.liberainformatica.it/forum/showthread.php?tid=1031&pid=6204#pid6204
*    How people steal and how not to get robbed. ( pickpockets )
        https://youtu.be/of0KkFW4T1o?t=368
*    If you can, avoid public wi-fi. If you need use, read about VPN https://en.wikipedia.org/wiki/Virtual_private_network
*    Never charge your phone via usb in a public place. By usb data may flee.
         Or use your own and special cable that can not transmit data.
*    Do not reveal things on social networking sites and on other websites, which you do not want to reveal.
        Short film which aims to raise awareness https://www.youtube.com/watch?v=_YRs28yBYuI
*    Where you can check data leaks ?   haveibeenpwned.com
*    Backup your important files and store in a safe place at home,
        because the hardware is not eternal, may break.



**FILES**

*    Read why we use sgid and why it can be dangerous
*    Read how find files with incorrect permissions and how find files with sgid
*    Read about chown , chmod 
*    Read about AIDA ( Advanced Intrusion Detection Enviornment )
        About AIDA http://www.liberainformatica.it/forum/showthread.php?tid=1031&pid=6185#pid6185
        Toy - intrusion detection  http://www.liberainformatica.it/forum/showthread.php?tid=1031&pid=6177#pid6177
*    Read what it is Access Control Lists
*    Read how to check the changed packages
*    Read how to check system logs and how to quickly find faults and how create alerts
*    Update the system systematically if possible, because a lot of attacks already use detected and repaired vulnerabilities.
*    Read why untested packages from outside the repository can be dangerous
*    Read why we use programs with a closed source code and why can be dangerous
*    Read about chkrootkit and rkhunter
*    Some programs allow disable scripts from settings, when you trying open
        .pdf , .doc , .xml , ... files. ( scripts may sometimes be dangerous )


**IF YOU ARE A PROGRAMMER**

*    Read about attacks on environmental variables
*    Read about attack on input files
*    Read about Validating Sanitizing and Escaping User Data



**TOOLS**

*     Against malicious software ( **Rkhunter , Chkrootkit , ClamAV** , http://www.virustotal.com )
*     Measurement of disk damage --> SMART ( **smartctl , GSmartControl** )
*     Measurement of RAM damage ( **Memtest86 , Memtest86+** ) 
*     Protection against fork bomb or system slowdown ( **nice and renice , ulimit , nproc** )
*     App which show the result measurement, like: cpu, ram consumption, temperatures, number of running processes, UPS status , ...
But remember that the app works hard for you and can be also cause faster wear of computer hardware.
( **Conky , top , htop , mate-system-monitor** )
*     Live-cd with several dozens of utility and diagnostic programs (** Ultimate Boot CD** )
*     Displaying active TCP connections ( **netstat** )
*     Detection of slow system startup ( **logs inside /var/log/ , bootchart , bootchart2** )
          video about boot: https://youtu.be/gIK1he6Ocpg
          Bootchart not have prep script to add option to grub, need edit menu.lst hand.
          For working bootchart we need add init=/sbin/bootchartd option to menu.lst For example
```
title linux
kernel (hd1,5)/boot/vmlinuz BOOT_IMAGE=linux root=LABEL=PCLos32 init=/sbin/bootchartd splash quiet resume=UUID=cca533ad-b070-4a2e-b0a6-bfc087ef6950 vga=788
root (hd1,5)
initrd /boot/initrd.img
```
          We need only reboot computer and chart from boot will in /var/log/bootchart.png 
          If you don't have, check /lib/bootchart/tmpfs and permissions for this folder.
*     Other website about security ( archlinux wiki )
         https://wiki.archlinux.org/index.php/Security


[hr]

**Changes:**
*- Post was rebuilt and have a few things added  22.07.2018*


#===========================
# Toys - intrusion detection

**1. notify-send**
```
su - user_name -c ' export DISPLAY=:0.0 ; /usr/bin/notify-send -t 10 -i gtk-dialog-info " ROOT LOGGED! `who` "'
```
This command will show information on the screen,
 but when is at the end of the file **/root/.bashrc** 
will show information when someone logs to root.
** Of course **user_name** you need change inside command*
** You can change Display, example how read variable from terminal*
```
 echo $DISPLAY
:0
```
** Of course this will not work for example, when you use gksu*

#===========================


Intrusion Detection
http://www.linuxsecurity.com/docs/SecurityAdminGuide/SecurityAdminGuide-7.html

In the video there is
```
# rpm -Va
```
( explained in the second link and in "man rpm" )
This is not an ideal method, 
when you like change the system file settings
and later check which files are edited, but not by you.
Because there will be many edited files
Exist better tools,

for example AIDE  http://aide.sourceforge.net/
AIDE constructs a database of the files specified in aide.conf.
You must keep the data base away from the system and in a safe place.
When you want check changes inside system (for example) after month,
then you need create new database, download old  database and check the differences.
Best way is use **aide** from live-usb and save download on live-usb.

1. AIDE have config in **/etc/aide.conf** and you can check
2. Creating a database ( # = root account )
```
# aide --init
```
3. We need move the database and create a new database for comparison.
```
# mv -v /var/lib/aide/aide.db.new /var/lib/aide/aide.db
# aide --init
```
4. We will check changes between **aide.db.new** and **aide.db** files.
```
# aide --check
```

After update system, to create **aide.db.new** file you can 
```
# aide --update
```
*
Edited.
- 24.11.2018*

#=====================

**Honeypots**
https://en.wikipedia.org/wiki/Honeypot_(computing)
Honeypot this is the kind of bait to catch the intruder.
Are different types, for various tasks. 
For network research and protection, system protection 
and maybe something else.

I wanted to write about email protection.
For example,
You've got or bought a game and and you got message  email 
with key to game.
But link to game, you can open only once.
If someone reads your messages and open links before you,
you will not get the code.
Then you know that, someone reading your messages 
and you need change password email.

In a similar way you can create own email message to detect intruders.
The message should be attractive to read, but " not must / does not have to " be real.
Most email addresses do not allow tracking of IP login,
but you can create and send ( from other email ) message with link,
 which will track clicks / visits. 
( link with tracking, you can create with bitly.com, goo.gl or other website. )


#=====================
**inotifywait**  /  **inotifywatch**

Useful tools to observe file changes without increasing system load.

Example script with **inotifywait**
```
#!/bin/sh
while :
do
	inotifywait --timefmt '%H:%M:%S' --format '%T %w %e %f' /etc/group 2>&1 | tee -a inotify.log
	sleep 1
	notify-send -t 0 Warning "Someone or something watching file /etc/group."
done
```

Issues with "while":
- Opening a file in the editor may cause several actions instead of one.
- Editing a file in the editor may crash the program with an error "IGNORE"
For me  helped " sleep 1 " inside script.

More in 
**man inotifywait** 
**man inotifywatch**
#=====================

**Security Quick-Start HOWTO for Linux**
This document is old, But some advice can be helpful.
http://tldp.org/HOWTO/Security-Quickstart-HOWTO/index.html

**Other guides:**
* " prevent unprivileged users from viewing dmesg  "
https://www.cyberciti.biz/faq/how-to-prevent-unprivileged-users-from-viewing-dmesg-command-output-on-linux/
*  " recommendations and best practices for hardening "
https://wiki.archlinux.org/index.php/Security
* " Linux Privilege Escalation "
https://payatu.com/guide-linux-privilege-escalation/

**Tools:**
* " Intrusion Detection Tools "
https://www.alienvault.com/blogs/security-essentials/open-source-intrusion-detection-tools-a-quick-overview
* " security-auditing tools "
http://www.linuxsecurity.com/content/view/164493/171/
* " security tools (top 100) "
https://linuxsecurity.expert/security-tools/top-100/

#=====================
