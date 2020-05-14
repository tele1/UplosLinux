# How to create your own repository in PCLinuxOS

1. Create own place for repository.
For example i created repository folder in home folder
2. Create folder which will contained ... .rpm files, folder must have a a prefix " RPMS. "
For example inside repository folder I created folder RPMS.32bit.
3. Put rpm files inside RPMS.folder_name .
My example loks like this:

```
home
 └── user 
      └── repository
             ├── RPMS.32bit
             ├── libopenssl_1.0.0-arch32-1.0.2g-1pclos2016.i586.rpm
             ├── libopenssl-arch32-devel-1.0.2g-1pclos2016.i586.rpm
             └── openssl-arch32-1.0.2g-1pclos2016.i586.rpm
```

4. Open terminal / konsole and copy and paste this line,
```
genbasedir --flat --bloat --progress /home/your_name_user/repository 32bit 
```
 then edit path to your repository and click **Enter**
You should see something like this: 

```
$ genbasedir --flat --bloat --progress /home/user/repository 32bit
Components: 32bit
Processing pkglists... 32bit 0003/0003 [done]
Processing srclists... [done]
Updating component releases... 32bit [done]
Creating global release file... [done]
Appending MD5Sum... 32bit
All your base are belong to us!!! [done]
```
directory structure inside repository folder looks now this 

```
$ tree
.
├── base
│   ├── pkglist.32bit
│   ├── pkglist.32bit.bz2
│   ├── release
│   └── release.32bit
└── RPMS.32bit
    ├── libopenssl_1.0.0-arch32-1.0.2g-1pclos2016.i586.rpm
    ├── libopenssl-arch32-devel-1.0.2g-1pclos2016.i586.rpm
    └── openssl-arch32-1.0.2g-1pclos2016.i586.rpm
______________________
2 directories, 7 files
```

**Now you have ready repository !** :D
TIP: More option for genbasedir you can find with command
```
genbasedir --help
```

TIP: For .src.rpm files you can create folder with prefix SRPMS. , for examle: SRPMS.32bit


# How add repository to Synaptic ?

1. Open Synaptic
2. Click: Settings --> Repositories
3. At the bottom, click New
4. Fill in empty fields.
For example I completed like this: 

<blockquote>URI:           file:/home/user/
Distribution:    repository 
Section(s):     32bit</blockquote>

TIP: If you trying add local repository on hard drive, path should begin like file:/home/user/
If you have repository on the web, path should begin like http:/ /server/path

5 Click OK (the window closes and the changes should be saved). You can check the active list, click: Settings --> Repositories (in Synaptic)
6. In the upper left corner inside Synaptic, you have the icon Refresh, click it to refresh and load new repositories.
7. Ready ! :D 


# How refresh own repository ?

If you want add or remove .rpm files from repository, You need refresh database.

1. You use the command as before, for example:

```
genbasedir --flat --bloat --progress /home/your_name_user/repository 32bit
```

2. Ready, your repository database is refreshed, now close Synaptic if you have open, then you can open Synaptic again, click Refresh icon and use. 

# **TIPS:**
1. If you want refresh repository one command ...
- create a script in /usr/local/bin/ with name uprep

```
#!/bin/bash
if  $EUID -ne 0 ; then
     echo " Warning: This script must be run as root! "
     echo " Try again as root, the end. "
fi
apt-get clean;
genbasedir --flat --bloat --progress /home/user/repository 32bit ;
apt-get update
```

- change path and section name if you have other,
- give script execution rights
- and ready, run command from terminal to check: /usr/local/bin/uprep or just uprep

2. Read also:
http://apt4rpm.sourceforge.net/web-apt-rep.html
http://apt-rpm.org/reposetup-native.shtml#simple-repo

Date of publication: 18.04.2018
- added for backup, because last time other web had problem.
- guide is for check in free time.
