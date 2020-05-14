How to downgrade a package? A good system, in order to trick synaptic, is to use "rversion".
At the top of of the spec file you inserted:
%define rversion your_number_version
Then you can add a +1 to %mkrel
Remember to change in the Source and %prep section from %{version} to %{rversion} 


The **second way** : with **smart** package manager
if older package exist in repository:
- Install **smart** package manager from Synaptic
- Open Smart and configure / edit repository
Edit --> Channels 
http://www.liberainformatica.it/forum/showthread.php?tid=342&pid=2421#pid2421
- To refresh, click first icon on left "Update channels"
- Downgrade a package,
( select, from right mouse, click "Install" , click second icon from left "Apply marked changes" )
  and ready.



# **The third way**.

We can not downgrade package from
Synaptic , smart package manager, apt
when first obsolete second.
For example
```
# apt-get install -f gnome-icon-theme
...
The following packages have unmet dependencies:
  adwaita-icon-theme: Obsoletes: gnome-icon-theme
E: Broken packages
```
But exist way, install by hand with rpm.


So, 
1.  Download needed to install packages ( from repository, by hand )
2. Remove new package without dependencies
```
 rpm -e --nodeps adwaita-icon-theme 
```
"rpm -e --nodeps" will remove package, but without remove dependencies.
( It is useful when the package want remove half system :D )

Now we need fix broken packages 
- install needed packages with rpm

Example:
```
[root@localhost test2]# ls
gnome-icon-theme-3.14.0-1pclos2015.noarch.rpm           
gnome-icon-theme-symbolic-3.12.0-1pclos2014.noarch.rpm  

[root@localhost test2]# rpm -ivh gnome-icon*
Preparing...                ########################################### [100%]
   1:gnome-icon-theme       ########################################### [ 50%]
   2:gnome-icon-theme-symbol ########################################### [100%]

```
And now I have downgraded  packages
from adwaita-icon-theme to gnome-icon-theme and gnome-icon-theme-symbolic.


# **Or maybe more safer** ( if you want remove kernel, glic )
```
[root@localhost test2]# rpm -ivh --force gnome-icon*
Preparing...                ########################################### [100%]
   1:gnome-icon-theme       ########################################### [ 50%]
   2:gnome-icon-theme-symbol ########################################### [100%]

[root@localhost test2]# rpm -e --nodeps adwaita-icon-theme

[root@localhost test2]# rpm -ivh --force gnome-icon*
Preparing...                ########################################### [100%]
   1:gnome-icon-theme       ########################################### [ 50%]
   2:gnome-icon-theme-symbol ########################################### [100%]

```
- Here first I installed needed package, 
then I removed newer package
and then I reinstalled need package ( if needed file was removed, reinstall package fix it ).
