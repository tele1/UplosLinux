# Mini guide for Iptables firewall.

Firewalls can be divided into network and software.
Iptables supports only network and it is a bit complicated.
Other Internet firewalls: 
**ufw** - with Gufw graphical interface
**shorewall** - graphical interface is inside UPLOS Control Center

Iptables consists of several tables. 
By default and most often is used "filter" table.
And "filter" table is divided into chains:
 **INPUT** - incoming data
 **FORWARD** - data redirection
 **OUTPUT** - outgoing data


# Example rules for desktop.

First check it:
http://ask.xmodulo.com/disable-ipv6-linux.html

It will give you knowledge:
- how to check own local IP 
( if you want to check the external IP, you can check on a special website)
- how looks and what type of IP you use ( IP v4 , IP v6 )
- name of the connection ( eth , wlan , lo )
( eth - ethernet )
( wlan - wireless local area network )
( lo - these are internal connections )

Default settings for iptables you can check and looks like this:
```
$ iptables -L

Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```
which means that all connections are open and nothing is blocked for IP v4
For IPv6 you need use **ip6tables** command, instead iptables.

Your task is to block all unnecessary Internet connections.
And unblocking the connections needed.

Example rules for desktop for IP v4
( IPv4 and IPv6 are are not compatible, so the firewall rules may look different )
```
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
iptables -A INPUT -p icmp --icmp-type 8 -j LOG --log-prefix "PING: " --log-level 4
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

```

If you want to know what it does, read the guides, or paste each line separately on the web page
https://explainshell.com/explain?cmd=iptables+-A+INPUT+-p+icmp+--icmp-type+echo-request+-j+DROP

Copy each line and paste into the terminal separately. ( from root )

**TIP:**
*Instead of running each line separately, you can run all commands at the same time in the script
Save all rules to file, and add at the beginning*
```
#!/bin/bash
```
*then give the appropriate permissions if you need, and run this script from terminal.*

Then check rules, should look like this
```
# iptables -L
Chain INPUT (policy DROP)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere            
DROP       all  --  anywhere             anywhere             ctstate INVALID
LOG        icmp --  anywhere             anywhere             icmp echo-request LOG level warning prefix "PING: "
DROP       icmp --  anywhere             anywhere             icmp echo-request
ACCEPT     all  --  anywhere             anywhere             state RELATED,ESTABLISHED

Chain FORWARD (policy DROP)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination 

```
```
# iptables -S
-P INPUT DROP
-P FORWARD DROP
-P OUTPUT ACCEPT
-A INPUT -m conntrack --ctstate INVALID -j DROP
-A INPUT -p icmp -m icmp --icmp-type 8 -j LOG --log-prefix "PING: "
-A INPUT -p icmp -m icmp --icmp-type 8 -j DROP
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
```

*# --> it just means that the command was run as root*
These settings are saved only until the next computer start.
To save the settings permanently inside UPLOS, you must run the command from root.
```
service iptables save
```

It is also important to distinguish the options
* --append  -A
 --insert  -I*
because rules with **-I** are they are performed first.
https://www.youtube.com/watch?v=eC8scXX1_1M

**Warning:**
The order of the rules is very important,
because firewall works like a marble machine.
1. If you want to write to the log one of the rules "drop",
this means that the "log" rule must be earlier.
Another way is to create a special table.
In the example above, "drop" must first be followed by "accept".
Otherwise the "drop" rules don't work,
because the connection will already be accepted by the first rule.


Manuals
1.  **iptables --help ** (command)
2.  **man iptables** (command)
3. https://en.wikibooks.org/wiki/Communication_Networks/IP_Tables
4. https://www.netfilter.org/documentation/index.html#documentation-howto

On the internet you will find a lot of sample rules.
If you looking example script or you want build own, check this:
https://github.com/tele1/Mur

Something else " DNS Tunneling Explained "
https://www.youtube.com/watch?v=q3dPih_8Cro
Therefore, it's best to allow DNS in the firewall only for a specific IP or limit the amount of data or both.
And log  other connections.

**Edited**
2019-08-29  --> fixed link to github
2019-09-09  --> rules order corrected
