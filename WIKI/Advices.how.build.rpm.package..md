# Advices are directed to developers,
but may be helpful to other users.


**1.** Before compiling the package 
- should be checked dependencies this package, and compiled and installed if newer.
- only advice, build rpm on virtualbox machine ( more safe )
think about your own safety: firewall, passwords, sandbox.
- try use "pkgutils-thunar" in Thunar file manager ( life can be simpler )
- if you need Fedora guides are very useful, but we don't use {?dist} tag in spec file.
- if you can, try use english language on own system, then you don't need translate the errors,
 when you looking help and some of the scripts working better.

        ( This will avoid many errors compilation )


**2.** New distributions are trying to optimize their operation under the new instance. graphics cards, 
   forgetting even properly configured hardware capabilities somewhat older.

    These options are included in the **./configure** and **cmake ..**
EXAMPLE:
<blockquote>./configure	
     --disable-static \
     --enable-dri \
     --enable-glx \
     --enable-glx-tls \
     --with-dri-drivers="i915,i965,nouveau,r200,radeon,swrast" </blockquote>
  So I suggest sometimes **check old and new .spec** file for more important packages like:
   ** gcc, mesa, codecs ...**


**3.** Sometimes a codec does not work after upgrade, all dependencies should be rebuilt with main package and it should fix issue.


**4.** Packages which contain scriptlets (%post , %preun  , %postun ...) 
  for example desktop environments (more precisely session packages) should be tested with command:
```
rpm -qp --scripts name_package
```
 and output from command should not be empty. 
If your output is empty, you had deficiencies in the system (missing packages) when you build this package.
Short example from installed package:
```
$ rpm -q --scripts mate-session-manager
postinstall scriptlet (using /bin/sh):
if [ -x /usr/sbin/fndSession ]; then /usr/sbin/fndSession || true ; fi 
```


**5.** Change file **~/.rpmmacros** is needed

```
%_topdir /home/your_nick/src/rpm
%_tmppath /home/your_nick/src/tmp
%packager your_nick
%distribution UPLOS
%distsuffix uplos
%vendor UPLOS
```

( packages which have "pclos2015.i586.rpm" , contain  " **%distsuffix uplos2015** " )



**7.** Sometimes .spec file have line " **Packager:** ... " , 
```
Name:		h...
Version:	1.9.0
Release:	%mkrel 2
License:	GPLv2+
Packager:       XXX
Group:		Emulators
```
 We don't  want edit this line in future, so please remove this line.
More important is edit **%changelog** at the end of the file
 and what we will see from command in terminal
```
rpm -qpi name.rpm
```
or for installed package
```
rpm -qi name
```


**8.** Please, if you can, try sometimes check, if source code is original.
especially if you've downloaded from other distribution.
- Download new orginal source code, version which you need check and unpack.
- Unpack old source code from src.rpm
- Check two directories with command
```
diff     -Naur        orinal_source_code     src_source_code   >   check.patch
```
a) If patch is empty, source code is the same, so is OK.
b) If you can not open patch ( is binary file ),  this source code is not safe    ( maybe picture is added )
c) If you see something,  this source code is not safe

Why ?
In src.rpm we should not edit source code hand, 
if we need edit, we should copy source, edit and then create patch like above, and in spec file we can apply patch.


**9.**  First path for build was, something similar:
```
/usr/src/SOURCES
/usr/src/SPECS
/usr/src/BUILD
/usr/src/RPMS 
/usr/src/SRPMS 
```
Now we use /home/user_name/scr/ ...
rpm sometimes show this user path and user group from package, 
especially when the bad links are typed.  ( I don't know why )
So,  if you feel fear, you can create "build" account for build packages.


**10.** If you create own first package and you have **.la** files,
please remove all static libs.

.bin -- binary file
.so -- symlink to shared lib
.so.1.0.0 -- shared lib ( file is dynamic library )
.a -- static lib
.la -- static lib (static libraries used by the GNU "libtools" package)

http://stackoverflow.com/questions/12237282/whats-the-difference-between-so-la-and-a-library-fileses

For disable .a files
```
./configure --disable-static
```

For remove .la files we use inside %install section
```
find %{buildroot} -regex ".*\.la$" | xargs rm -f --
```
or
```
rm -f %{buildroot}%{_libdir}/*.la
```

Example:
```
%install
rm -rf %{buildroot}
%makeinstall_std

find %{buildroot} -regex ".*\.la$" | xargs rm -f --
```

Guides:
Packaging_Static_Libraries  https://fedoraproject.org/wiki/Packaging:Guidelines#Packaging_Static_Libraries
Devel_Packages  https://fedoraproject.org/wiki/Packaging:Guidelines#Devel_Packages


**11.** Please don't try create files from %post %postun.

Why?
Because this files are not in %files section,
so if you want check **rpm -qf file** , just not will work

Example in wrong spec:
```
%post
# update shared library cache
/sbin/ldconfig

cd %{_datadir}/wallpapers/
[ ! -d Default ] && mkdir Default || rm -rf Default && mkdir Default
cd Default

cat > metadata.desktop << "EOF"
[Desktop Entry]
Name=Default
EOF
```

```
# rpm -qf /usr/share/wallpepers/Default/metadata.desktop
file /usr/share/wallpepers/Default/metadata.desktop is not owned by any package
```

Example with the correct way:
```
Source:		%{name}-%{version}.tar.xz
Source2:		metadata.desktop

...

%install
%__rm -rf %buildroot

mkdir -p ${RPM_BUILD_ROOT} /usr/share/wallpepers/Default/
install  -m 755 %{SOURCE2} ${RPM_BUILD_ROOT}/usr/share/wallpepers/Default/metadata.desktop

...

%files
%defattr(-,root,root,-)
/usr/share/wallpepers/Default/metadata.desktop

```

```
# rpm -qf /usr/share/wallpepers/Default/metadata.desktop
wallpaper-theme-uplos-1-1uplos2016
```


**12.** Clang
If we update clang, this may broke packages which use clang.

So after update clang,
we should check and maybe rebuild packages ( packages which use clang for run ) 


**13.** Port PCLinuxOS src.rpm to UPLOS.
- System packages, scripts require careful checking. ( hell dependencies )
- We are obliged to change all the names from PCLinuxOS to UPLOS

Simplest way to do it via .spec file.  ( change name )
- Change needed names in .spec file
- Check src.rpm files which names change
- Add needed lines to at the beginning "build" section in .spec file
for example
```
%build
find . -type f | xargs sed -i  "s/pclinuxos/uplos/g"
find . -type f | xargs sed -i  "s/PCLinuxOS/UPLOS/g"
find . -type f | xargs sed -i  "s/PCLOS/UPLOS/g"
find . -type f | xargs sed -i  "s/pclos/uplos/g"
```
More in http://www.liberainformatica.it/forum/showthread.php?tid=543
- After that, test package how working.
( This has big advantages: time and clean source code before build package )


**14.** Having your own gpg key is required,  to sign packages.
Tutorials:
- http://www.liberainformatica.it/forum/showthread.php?tid=691&highlight=gpg
- http://www.liberainformatica.it/forum/showthread.php?tid=533&highlight=gpg


**15.** We take packages from  https://ftp.nluug.nl/os/Linux/distr/pclinuxos/pclinuxos/srpms/SRPMS.pclos/
to rebuild for uplos. This is the main basis
If you want help rebuild packages write to
**adrianomorselli**  http://www.liberainformatica.it/forum/member.php?action=profile&uid=75


#=======================================================================
** Useful commands:**

- When we want find or check installed package
```
rpm -qa | grep name_package
```
or when we want search in repository
```
apt-cache search name_package
```

Warning:  apt-cache use file "pkglist. ..."  from created repository, 
and his possibilities depend, how the repository has been done. (with which special options)

- When we want search file in repository, to check which package provide file
```
apt-cache searchfile  name_file
```

- When we want find which packages use our package.    
```
apt-cache whatdepends name_package
```

- When we want find what need to install our package.
```
apt-cache whatprovides name_package
```

- When we want check what do macro
```
rpm --eval %name_macro
```
or
```
rpm --eval=%name_macro
```

- Other examples ...
https://www.tecmint.com/20-practical-examples-of-rpm-commands-in-linux/


** Useful links:**

rpm commands           http://www.cyberciti.biz/howto/question/linux/linux-rpm-cheat-sheet.php
Build rpm package:
_________  1. PCLinuxOS  http://pclinuxoshelp.com/index.php/Packaging_for_PCLinuxOS
_________  2. PCLinuxOS  magazin http://pclosmag.com/html/Issues/201007/page15.html
_________  3a. Fedora  https://fedoraproject.org/wiki/How_to_create_an_RPM_package
_________  3b. Fedora  https://fedoraproject.org/wiki/Packaging:Guidelines?rd=Packaging
_________  4a. Mageia   https://wiki.mageia.org/en/Packagers_RPM_tutorial
_________  4b. Mageia  https://wiki.mageia.org/en/Packaging_guidelines
_________  5. max-rpm  http://www.rpm.org/max-rpm/index.html
_________  6a. Mandriva  http://archive.openmandriva.org/wiki/en/index.php?title=RPM_packaging_tutorial
_________  6b. Mandriva http://archive.openmandriva.org/wiki/en/index.php?title=Development/Tasks/Packaging/Tools/RPM/AdvancedHowto
_________  6c. Mandriva  http://archive.openmandriva.org/wiki/en/index.php?title=Development/Tasks/Packaging
_________  6d. Mandriva  http://archive.openmandriva.org/wiki/en/index.php?title=Problems_and_issues_with_packaging
_________  6e. Mandriva  http://archive.openmandriva.org/wiki/en/index.php?title=Installing_software_from_source_code
_________  7. Uplos http://www.liberainformatica.it/forum/showthread.php?tid=327&highlight=tutoriale  
_________  8. Making an empty RPM:
_________  __  https://ttboj.wordpress.com/2015/08/11/making-an-empty-rpm/
_________  9. Making an RPM without source code:
_________  __  http://www.pclinuxos.com/forum/index.php/topic,124179.msg1038019/topicseen.html#msg1038019

How to sign your custom RPM package with GPG key    https://gist.github.com/fernandoaleman/1376720
Packaging Environment   http://pclosmag.com/html/Issues/201007/page16.html
How to remove unwanted 'devel' packages    http://www.liberainformatica.it/forum/showthread.php?tid=371

Create own repository  http://www.liberainformatica.it/forum/showthread.php?tid=1151y
Rsync repository            http://www.liberainformatica.it/forum/showthread.php?tid=331


Where looking spec, patch in other distributions:
1. https://www.archlinux.org/packages/?sort=&q=cdrdao&maintainer=&flagged=
2. http://ftp.nluug.nl/os/Linux/distr/
3. http://rpm.pbone.net/
4. https://apps.fedoraproject.org/packages/
5. http://software.opensuse.org/search
6. http://sophie.zarb.org/distrib
7. http://www.linuxfromscratch.org
8. http://software.opensuse.org/search
9. http://www.filewatcher.com/
10. https://repology.org/


Kernel patches
1. CK - https://en.wikipedia.org/wiki/Con_Kolivas
 _ http://ck-hack.blogspot.com/
 _ http://ck.kolivas.org/patches/
2. XanMod Kernel https://www.phoronix.com/scan.php?page=news_item&px=XanMod-Kernel-Tests
 _ https://xanmod.org/
3. Other list
_ https://wiki.archlinux.org/index.php/Kernel#Major_patchsets
4. Budget Fair Queueing (BFQ)
_ https://algo.ing.unimo.it/people/paolo/disk_sched/
5. Kernel Self Protection Project/Recommended Settings
_ http://kernsec.org/wiki/index.php/Kernel_Self_Protection_Project/Recommended_Settings
6. grsecurity ( available to their paying customers )
_ https://en.wikipedia.org/wiki/Grsecurity
7. kernelcare
_ https://en.wikipedia.org/wiki/KernelCare

Standards:
1.  Linux Standard Base (LSB)          http://refspecs.linuxfoundation.org/lsb.shtml
2.  Filesystem Hierarchy Standard  (FHS)     http://refspecs.linuxfoundation.org/fhs.shtml
3.  Link to whole list Standards        http://refspecs.linuxfoundation.org/
4. Freedesktop Specifications (FS)  https://www.freedesktop.org/wiki/Specifications/



Software news:
1.  http://distrowatch.com/news/dwp.xml

 Vulnerability news.
1.  http://www.cvedetails.com/vendor/33/Linux.html


Default permissions for packages
-rw-r--r-- 	644   Read and write access for owners and readings for all site visitors.
Change permissions
```
chmod 644 package.rpm
```




*Last topic edited: 9.09.2018*
- added link to arch wiki about kernel
- removed 1 old link
- added "5. Budget Fair Queueing (BFQ)https://algo.ing.unimo.it/people/paolo/disk_sched/ "
Edited 2019-08-29
-added link to Kernel Self Protection Project




#======================================

## About **rpmdevtools**

I do not use rpmdevtools, You use them at your own risk.

How find ?
```
$ rpm -ql rpmdevtools | grep bin
/usr/bin/annotate-output
/usr/bin/checkbashisms
/usr/bin/licensecheck
... 
```

More info in man " DESCRIPTION "
For example:

```
man rpmdev-newinit
```
<blockquote>DESCRIPTION
       rpmdev-newinit generates new init script from a template</blockquote>

```
man rpmdev-newspec
```
<blockquote>DESCRIPTION
       rpmdev-newspec generates new rpm .spec files from templates.</blockquote>

```
man spectool
```
<blockquote>DESCRIPTION
       Spectool  is  a  tool  to  expand and download sources and patches from
       specfiles.</blockquote>

Spectool is not supported, because 
- we UPLOS, PCLinuxOS not always use links inside .spec file.
- we store, source code inside src.rpm file for safety, if it disappears from the official site
and we don't need download old source code for rebuild, if working.
Url link is needed for download source code from spec file.

Spectool read web links from .spec files
```
 $ spectool -A pclos-desktop-file-utils.spec
Source0: http://freedesktop.org/software/desktop-file-utils/releases/desktop-file-utils-0.22.tar.xz
Source1: desktop-file-utils.el
Source2: update-desktop-database.filter
Source3: update-desktop-database.script
Patch0: desktop-file-utils-0.22-registered-categories.patch
```

And can download
```
$ spectool -g pclos-desktop-file-utils.spec
Getting http://freedesktop.org/software/desktop-file-utils/releases/desktop-file-utils-0.22.tar.xz to ./desktop-file-utils-0.22.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   356  100   356    0     0    579      0 --:--:-- --:--:-- --:--:--   908
100  127k  100  127k    0     0  60668      0  0:00:02  0:00:02 --:--:-- 99565
```

Instead of this, inside .spec files you can find for example
<blockquote>%prep
#Here we go!
%ifarch %{ix86}
cd %_builddir
wget -nc http://ftp.mozilla.org/pub/firefox/releases/%{realver}/linux-i686/en-US/firefox-%{realver}.tar.bz2
tar -xvjf firefox-%{realver}.tar.bz2
%else
cd %_builddir
wget -nc http://ftp.mozilla.org/pub/firefox/releases/%{realver}/linux-x86_64/en-US/firefox-%{realver}.tar.bz2
tar -xvjf firefox-%{realver}.tar.bz2
%endif</blockquote>
This is more comfortable, because source code is download automatically when you want build package.

Unfortunately, it also has disadvantages,
is more difficult to find url link with script which will automatically update the package.

Why is not used 
```
 wget -nc  %{Source0} 
```
Maybe  is a problem with writing a valid link, because source package sometimes have 
another name, I'm not sure.
Maybe  because before build, some kind of script checks name package and 
if the files are in a directory, spec file. 

If you want, you can test whether it will work.
```
%ifarch %{ix86}
    Source0 http://ftp.mozilla.org/pub/firefox/releases/%{realver}/linux-i686/en-US/firefox-%{realver}.tar.bz2
%else
    Source0 http://ftp.mozilla.org/pub/firefox/releases/%{realver}/linux-x86_64/en-US/firefox-%{realver}.tar.bz2
%endif

%prep
cd %_builddir
wget -nc %{Source0} 
```
Not working ... :(  
Correct way should be
- rpmbuild should find  package by name and version inside ~/src/rpm/SOURCES/ automatically
- everybody should use  " Source0: http://path/to/download/%{name}-%{version}.tar.xz "
And here something works bad, rpmbuild uses **Source0:** to find source code
 on local hard drive, which can have other name, than on website.
- if you don't use url path, should be " Source0: %{name}-%{version}.tar.xz "
- %{source0} or something should show also real url,
https://fedoraproject.org/wiki/How_to_create_an_RPM_package#SPEC_file_overview

## **Example How Debug**
- You can use **echo**
<blockquote>%prep
cd %_builddir
echo %{SOURCE0}
echo %{Source0}
echo %{source0}
echo source0
echo %source0
echo %Source0
echo $Source0
echo "----"
echo %{S:0}
wget -nc %{SOURCE0}</blockquote>
  Part output from build:
<blockquote>+ cd /home/tele/src/rpm/BUILD
+ echo /home/tele/src/rpm/SOURCES/firefox-55.0.3.tar.bz2
/home/tele/src/rpm/SOURCES/firefox-55.0.3.tar.bz2
+ echo '%{Source0}'
%{Source0}
+ echo '%{source0}'
%{source0}
+ echo source0
source0
+ echo %source0
%source0
+ echo %Source0
%Source0
+ echo

+ echo ----
----
+ echo /home/tele/src/rpm/SOURCES/firefox-55.0.3.tar.bz2
/home/tele/src/rpm/SOURCES/firefox-55.0.3.tar.bz2 

+ wget -nc /home/tele/src/rpm/SOURCES/firefox-55.0.3.tar.bz2
/home/tele/src/rpm/SOURCES/firefox-55.0.3.tar.bz2: Scheme missing.
error: Bad exit status from /home/tele/src/tmp/rpm-tmp.mB6ETP (%prep)</blockquote>
And you see from "echo" output.
- You can also use ** exit 1 ** , like in bash script to stop build package.
- You can use **pwd** or other command

Spec file
<blockquote>%prep
cd %_builddir
pwd

exit 1</blockquote>
Output from build
<blockquote>+ cd /home/tele/src/rpm/BUILD
+ pwd
/home/tele/src/rpm/BUILD
+ exit 1
error: Bad exit status from /home/tele/src/tmp/rpm-tmp.x12Rad (%prep)</blockquote>

## **rpmlint**
Is also **not** supported from Uplos, this is for check errors inside src.rpm package
```
$ rpmlint  desktop-file-utils-0.22-4uplos.src.rpm
desktop-file-utils.src: W: strange-permission desktop-file-utils-0.22-registered-categories.patch 0664L

desktop-file-utils.src: W: source-or-patch-not-compressed bz2 desktop-file-utils-0.22-registered-categories.patch
```


Edited:
30.10.2017


#==================================

# **How build own kernel with PAE support **
PAE on wiki --> https://en.wikipedia.org/wiki/Physical_Address_Extension

On the uplos, manual building and launching can be difficult because the kernel should have several links for "draktools".
drak tools on wiki --> https://en.wikipedia.org/wiki/Drakconf
However rebuilding the package is quite simple.
1. Prepare kernel src.rpm for rebuilding.
2. Based on the installed and active kernel
(you can check used kernel " uname -a "), prepare a new config file.
- In a separate folder unpack source code of  kernel.
- In terminal paste 
```
make olddefconfig
```
```
make menuconfig
```
- In kernel menu, select  
<blockquote>Processor type and features ---></blockquote> 
then search and select
<blockquote>High Memory Support </blockquote>
and select a greater number of supported RAM.
- Leave the menu
3. Copy " .config " file (with dot it is hidden file).
4. Inside ~/src/rpm/SOURCES/ unpack patches-3.16.XX-1uplos.tar.xz
5. Paste  " .config " file in /src/rpm/SOURCES/patches-3.16.XX-1uplos/configs/
6. Rename  " .config " file to config-3.16.XX.pae
7. Pack again patches-3.16.XX-1uplos to tar.xz 
8. Do not forget in the .spec file change
```
%define build_kernel_default	1
%define build_kernel_bfs	0
%define build_kernel_pae	0
```
to
```
%define build_kernel_default	0
%define build_kernel_bfs	0
%define build_kernel_pae	1
```
9. Ready :-) , just rebuild the kernel.

# **Problems with drivers**
A newer kernel will require updating or rebuilding dkms drivers.
And sometimes graphic driver being tested does not want work.
If you are testing a closed driver, after error, system should automatically start the open driver.
And when you want run graphical applications with root privileges, you may seen:
```
Invalid MIT-MAGIC-COOKIE-1 keyError: cannot open display ':0'
```
Remove files
```
rm ~/.Xauthority
```
```
rm ~/.ICEauthority
```
and logout or reboot should help.
Also should work command from root, but only to next reboot
```
xhost +si:localuser:root
```

# **How change graphic driver in text mode?**
```
$ locate drak | grep bin
/usr/bin/XFdrake
```
Run /usr/bin/XFdrake
This working from window and terminal ( in tty and in save mode )
But choosing the right driver, for example Nvidia, can sometimes be difficult.
```
$ apt-cache search nvidia | grep dkms
dkms-bbswitch - bbswitch - Optimus GPU power switcher
dkms-nvidia-current - NVIDIA kernel module for GeForce 420 and later cards
dkms-nvidia304 - NVIDIA kernel module for GeForce 61xx to GeForce 79xx
dkms-nvidia340 - NVIDIA kernel module for GeForce 8xxx, 9xxx and 100 to 415 card
```
Here we have a small hint.
You see drivers nvidia340, nvidia304, and nvidia-current  this is 361 after check
```
$ rpm -qa | grep nvidia-cur
x11-driver-video-nvidia-current-361.28-2pclos2016
dkms-nvidia-current-361.28-2pclos2016
nvidia-current-cuda-opencl-361.28-2pclos2016
```
And you can check each driver on the nvidia website, for example for 340
https://www.nvidia.com/download/driverResults.aspx/77224/en-us
and in "Supported products" you have entire list of supported devices.
**How to change open driver to closed ?**
- If you have all the needed packages, when you trying change driver,
XFdrake will ask you if you want use closed driver.


#===================
Qmake works with qt3, qt4, qt5 and if it fails with "wrong qmake for qt5 found" open a terminal and become root
#update-alternatives --install /usr/bin/qmake qmake /usr/lib/qt5/bin/qmake 100
#update-alternatives --config qmake
Select qt5 

