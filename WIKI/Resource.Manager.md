Linux - Resource Manager

Example tutorial:    https://gerardnico.com/os/linux/limits.conf
Doc.    http://www.linux-pam.org/Linux-PAM-html/sag-pam_limits.html
# **
Attention !
This tutorial is for advanced users.
If you don't  know Linux very well, if you do not know the terminal, you don't know how use it,
probably this tutorial is not for you. 
Because badly chosen values can cause that you will only start the system in safe mode.
To restore or delete values you must know how to edit a file in text mode.**


In practice.
I will limit the number of running processes for the user

1. Check the number of running processes now.
```
$ ps -AL --no-headers | wc -l
530
```

2. Search for the maximum number possible.
```
#  cat /proc/sys/kernel/threads-max
55998
```
```
# cat /proc/sys/kernel/pid_max
32768
```
You can check also 
```
# ulimit -a
```

It is important. I need limit the number of processes for the user and leave a sufficient number for the administrator.
If I give too small a number, I will not be able to run the system. ( only I will can run system in  safe mode )
If you give too large a number, it may not work.

3. Edit file **/etc/security/limits.conf** from root
```
 nano /etc/security/limits.conf
```
and add new lines
for example I added like this
```
@user_name           soft    nproc           4000
@user_name           hard    nproc           20000
```

4. Reboot your own computer.

5. Test if it works.
Many users complain that the system hangs and not knowing why.
The system may not be able to record a failure.
This is example how to limit system resources to find a problem.
Now we do not have such problems.
But to be sure whether it works, we need create a problem.

One of the reasons why the limits are used is also fork bomb. https://en.wikipedia.org/wiki/Fork_bomb
We run it. :-)

- Run one terminal with administrator rights
- run a second terminal with user rights
- inside admin terminal check how many process is running
```
# ps -AL --no-headers | wc -l
529
```
- from user treminal run fork bomb ( attack system started )
```
$ :(){ :|:& };:
```
- inside root terminal check number running process
```
# ps -AL --no-headers | wc -l
4200
```
- after some time in the terminal of the user instead
```
bash: fork: retry: No child processes
bash: fork: retry: No child processes
bash: fork: retry: No child processes
bash: fork: retry: No child processes
```
you will see
```
bash: fork: Resource temporarily unavailable
bash: fork: Resource temporarily unavailable
bash: fork: Resource temporarily unavailable
bash: fork: Resource temporarily unavailable
```
This is the end of the attack.
- inside root terminal check number running process
```
# ps -AL --no-headers | wc -l
526
```
The test finished successfully, the system now works normally :-)

At the time of the attack operation You can try to check the cause.
You can try use **top**  https://en.wikipedia.org/wiki/Top_%28software%29
**htop**  https://pl.wikipedia.org/wiki/Htop
In this case it can be harder, my attack does not consume a lot of resources.
I used **pstree** for try counting the number of processes.
```
# pstree -a |  uniq -c | sort -nr | head -n 5
    984   |-bash
    664   |-bash
     90   |-bash
     88   |-bash
     65   |-bash
```
```
# pstree -a |  uniq -d | uniq -c |sort -nr | head -n 5
    229   |-bash
      1   |   |-saslauthd -m /var/run/saslauthd -a pam
      1   |           |   |   |-bash
```
It allows me see which identical processes are the most.
