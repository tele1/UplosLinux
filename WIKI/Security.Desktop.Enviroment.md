Tutorial is intended for testers.

1. Search **LISTEN** process. ( Command in terminal )
```
$ netstat -ntulp
Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name
tcp        0      0 0.0.0.0:47274                       0.0.0.0:*                       LISTEN      5031/mate-session
tcp        0      0 :::40271                               :::*                              LISTEN      5031/mate-session
```

Clean, basic  Desktop Environment should not listen from website.

Translate Adress:
:::80  -->  Any IPv6 address ,  for port 80 traffic
0.0.0.0:80  -->  Any IPv4 address,  for port 80 traffic
::1:80  -->   IPv6 localhost only,  for port 80 traffic
127.0.0.1:80  -->  IPv4 localhost only,  for port 80 traffic
192.168.0.1:80  -->  IPv4 address 192.68.0.1 (private network),  for port 80 traffic
*:*  -->  Any IP, for any port traffic

More about IP in
- https://en.wikipedia.org/wiki/IPv6_address
- https://en.wikipedia.org/wiki/IPv4#Addressing


2. Search the zombie.
```
$ ps aux | grep 'Z'
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
tele      1372  0.0  0.0      0     0 ?        Z    paź03   0:00 [xbrlapi] <defunct>
tele      7710  0.0  0.0  12784   984 pts/0    S+   14:29   0:00 grep Z
```

First process [xbrlapi] this is zombie.
Second process "grep Z" this is only own process grep with "Z" ( this is process is not important )


3. Search words 
" **error, fail , failed**, unable to start, not such file, not fond ... "
inside file  **~/.xsession-errors **
This file is very useful to find errors inside Desktop Environment.


4. Search  any other issue, error.


5. Search broken packages
http://www.liberainformatica.it/forum/showthread.php?tid=951


Edited.
22.03.2018
