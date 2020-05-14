

manuals
http://www.liberainformatica.it/forum/attachment.php?aid=170
http://www.liberainformatica.it/forum/attachment.php?aid=171


HOW TO MAKE TWO OR MORE VERSIONS OF GCC COEXIST IN THE SAME SYSTEM
Tutorial by Adrianomorselli

Sometime the recompilation of a package fails due partially to outdated dependencies and in part for we are using gcc_4.9, that is the current version of gcc, whilst several packages have been built with older versions.
Therefore, the question is: is it possible to make 2 different versions of gcc coexist in the same system?
The answer is: yes, as indicated @
https://gcc.gnu.org/faq.html#multiple.

For my trials I started from an italian tutorial in a fedora site @
http://doc.fedoraonline.it/Installare_un...d_installa
and I picked the source for gcc @
ftp://ftp.gwdg.de/pub/misc/gcc/releases/

Download the version you prefer (I took the 4.7.4, from now on I will refer to this one; if you take another, you have to modify accordingly).

Create a dir in your home, ~/gcc and inside create a subdir ~/gcc/build.
Unpack your gcc version in ~/gcc.
Open a shell in ~/gcc and digit:
$ ./configure \
$ --prefix=/opt/gcc47 \
$ --program-suffix=47 \
$ --enable-languages=c,c++

Resolve the eventual required dependencies (I have been asked for a couple of devel) and, accordingly with the given command, gcc will be installed in /opt/gcc47 in order to not interfere with the default gcc.

In the documentation I had found, it's available a lot of further references and settings for different enviromnments and languages like java and specifically for Fedora but I hope that what said up to now is enough for our needs.

In the current shell you can now digit
$ make
and wait. I had to wait up to 6 hours while the tutorial I had spoke of 30 minutes... Anyway everything ran correctly and I could digit (as root)
# make install

Once finished, there is yet something to do:
create as root a text file named /etc/profile.d/gcc47.sh (or accordingly) containing the following and make it executable and readable for all:
#!/bin/sh
GCC47_BIN=/opt/gcc47/bin
PATH=$GCC47_BIN:$PATH
export PATH
Now reboot and you'll have two running versions of gcc!"

At this point, how to use one or another version of gcc?
First of all be sure that all the required dependencies have been installed.
Open a shell in the dir containing the file .spec and digit
$ export CC=gcc47
$ export CXX=g++47
$ rpmbuild -ba file.spec
In this way you set an environment variable to use a compiler instead of the other one 
