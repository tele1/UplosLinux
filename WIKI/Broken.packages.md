# **1. Script - what it does?**

When you want test package, but you don't want install, you will add option "-s"
```
apt-get install -s package
```

When we want test all packages from "test" repository:
A) we need add section "test" to own list repository inside Synaptic.
B) then we need save all packages from "test" repository to file, then
```
apt-get install -s $(cat file)
```

So I do like this with script:
a) I add section "test" inside Synaptic, and "Reload/Refresh" repository, then I close Synaptic.
b) I run script from terminal to download list packages from  "test" section repository to file.
For this, in first window script, I leave the "test" section or I change "test" to other if I want try other section.
( but if I want try other section, before always I need add with Synaptic to repository )
c) then script do 
```
apt-get install -s $(cat file)
```
script save output to next file2, 
d) then script open this file2 with text editor to show output from command "apt-get install -s $(cat file)"
and from "apt-get -s dist-upgrade".

"apt-get -s dist-upgrade" - this is not the same what apt-get install -s ...
because "dist-upgrade" will try upgrade packages which are only installed inside Uplos


**3. Apt / Synaptic bugs.**
Apt is also not perfect, have bugs, so we need be careful.
For example:

```
apt-get install -s $(cat TMP/32bit.rpm.lista2)
libdc1394_12-devel: Conflicts: libdc1394-devel
E: Broken packages
```
Package is update to new package with other name, this is not error.

```
apt-get -s dist-upgrade
181 upgraded, 18 newly installed, 2 replaced, 3 removed and 0 not upgraded.
Conf adwaita-icon-theme broken
Obsoletes:gnome-icon-theme Obsoletes:gnome-icon-theme-symbolic  []
E: Conf Broken adwaita-icon-theme
```
This error exist when apt trying install but I don't know what it is "conf" or where.
You tested before package, Threepio and someone else and system working after upgrade.
Also from Synaptic or with "apt-get install" I don't see this error.

Synaptic when upgrade system, may overwrites files if they are in  the same place inside the system.
"Smart" package manager give warning then quite another package want replace files.

For me important inside "apt" output is:
- when it give other error
- when packages are removed 

Packages which are removed:
- may have broken dependencies,
- spec file bug 
- apt bug
- everything may be fine ( new package may remove old and not supported packages )


# **2. Script**
```

#!/bin/bash

# Licence: GNU General Public License v3
# Version 8

#==================================={
# dependencies
            RC='\e[0;31m' # Red Color
            NC='\e[0m' # No Color

            func_Error_Depend()
            {
                echo "======================"
                echo -e "${RC}    Error: Missing $1.
    Please install $1, before run script. ${NC}"
                echo "======================"
            }

[[ -z $(which lynx) ]] && { func_Error_Depend lynx ; exit 1 ;}
[[ -z $(which gtkdialog) ]] && { func_Error_Depend gtkdialog ; exit 1 ;}


#-----------------------------------{
funcError()
                {
GTKDIALOG=gtkdialog

export WindowError='
    <vbox>
        <frame>
            <hbox>
                <pixmap icon_size="6">
                    <input file stock="gtk-dialog-error"></input>
                </pixmap>
                <vbox>
                <text use-markup="true">
                    <label>"<span size='"'large'"' color='"'red'"'>'$"Error:"'
'"$1"' </span>"</label>
                </text>
                </vbox>
            </hbox>
        </frame>
        <hbox>
            <button>
                <label>'$"exit"'</label>
                <input file icon="exit"></input>
                <action>echo You pressed exit button</action>
                <action type="exit">Exit by button</action>
            </button>
        </hbox>
    </vbox>'

gtkdialog -c --program WindowError
exit 1
}
#-----------------------------------}
#===================================}


#---Checking exist dirs to work --{
aaa=`pwd`/TMP/
if [ ! -d $aaa ];
   then
   echo "TMP dir not found / will created."
   mkdir TMP
else
    rm -fvr TMP
    mkdir TMP
fi
#---------------------------------}


#============================================{
#------- CREATING THE LIST 32 BIT ----------------------------------{
#-----GUI------------------------{

function gui {
URI="http://ftp.nluug.nl/os/Linux/distr/"
DISTR="uplos/32bit"
OLD_SECTIONS=$(grep ^"rpm " /etc/apt/sources.list | cut -d" "  -f4-)
echo "test" > `pwd`/TMP/sections


export MAIN_DIALOG='
<window>
<vbox width-request="600">

<frame>
    <button>
		<input file icon="reload"></input>
		<label>Edit active sections.</label>
        <action>echo "2" > `pwd`/TMP/exit</action>
        <action type="exit">"Exit by button"</action>
    </button>
</frame>
<frame>
  	<text>
		<label>List active sections:</label>
  	</text>
    <entry>
		<default>'"$OLD_SECTIONS"'</default>
      <variable>ENTRY1</variable>
      <visible>disabled</visible>
    </entry>

</frame>
<frame>

  <text>
    <label>From which active section download list packages to test ?</label>
  </text>
  <entry>
    <default>'"$(cat `pwd`/TMP/sections)"'</default>
    <variable>ENTRY2</variable>
  </entry>
</frame>

  <hbox>


    <button cancel>
        <action>echo You pressed exit button</action>
        <action>echo "1" > `pwd`/TMP/exit</action>
        <action type="exit">"Exit by button"</action>
    </button>
    <button>
        <input file stock="gtk-apply"></input>
        <label>Start</label>
        <action>echo "$ENTRY2" > `pwd`/TMP/sections</action>
        <action>echo "0" > `pwd`/TMP/exit</action>
        <action type="exit">"Start by button"</action>
    </button>
  </hbox>
</vbox>
</window>
'
gtkdialog --program=MAIN_DIALOG
}


until [[ $EXIT == 0 ]]; do
	gui
	EXIT=$(cat `pwd`/TMP/exit)
	if [[ $EXIT == 1 ]]; then
		echo "Exiting"
    	exit 0 
	elif  [[ $EXIT == 2 ]]; then
		gksu "synaptic -r"
	fi
done
#-----GUI------------------------}
#============================================}


# Save `pwd`/TMP/sections to bash table  ------{
NUMBER_OF_WORDS=$(cat `pwd`/TMP/sections | wc -w)

    for i in `seq 1 $NUMBER_OF_WORDS`
    do
        DZIALY_32BIT[$i]=$(cat `pwd`/TMP/sections |  cut -d" " -f${i})
        echo ${DZIALY_32BIT[$i]}
        echo "----"
    done

LICZBA_DZIALOW_32BIT="${#DZIALY_32BIT[@]}"
# Save `pwd`/TMP/sections to bash table  ------}


#---------------{
# Check link to repository
aba=$(grep ^"rpm ${URI} ${DISTR}" /etc/apt/sources.list | wc -l)

if [ "$aba" -ne "1" ]; then
    ERROR="    rpm ${URI} ${DISTR}
    this repository is not active.
    Please check in Synaptic."
    funcError "$ERROR"
fi
#---------------}

echo " --> Number of sections: $LICZBA_DZIALOW_32BIT "

#======================================================{
# 1. ------- CREATING THE LIST 32 BIT PACKAGES ----
    for i in `seq 1 $LICZBA_DZIALOW_32BIT`
    do
        NUMER=$(($i))
        DZIAL=RPMS.${DZIALY_32BIT[$NUMER]}
        echo "$NUMER - $i - $DZIAL"


        #-- Check  repository sections --{
        TEST_SECTION=$(grep ^"rpm ${URI} ${DISTR}" /etc/apt/sources.list | grep ${DZIALY_32BIT[$NUMER]} | wc -l )

        if [ "$TEST_SECTION" -ne "1" ]; then

            ERROR="    ${DZIALY_32BIT[$NUMER]}
    this section is not active / not added.
    Please add and reload repository in Synaptic,
    you can later this section remove."
            funcError "$ERROR"
        fi
        #-- Check  repository sections --}


        lynx   -nolist -dump -width 120  ${URI}${DISTR}/$DZIAL/ \
        | grep "\[   \]" | awk -F" " '{ print $3}'  | \
        sed -e 's/google-chrome-stable_current_i386.rpm/google-chrome-stable-1.0-1/g' \
            -e 's/nomachine_4.4.12_12_i686.rpm/nomachine-1.0-1/g'  \
            -e 's/teamviewer_11.0.53191.i686.rpm/teamviewer-11.0.53191-1/g' | \
        sed -e 's/pclos.....i586.rpm//g' \
        -e 's/pclos.....noarch.rpm//g' \
        -e 's/.i386.rpm//g' \
        -e 's/uplos.....i586.rpm//g' \
        -e 's/uplos.....noarch.rpm//g' >> `pwd`/TMP/32bit.rpm.lista0
    done

echo " - LIST OF PACKAGES 32 BIT IS READY :)"

#=================={
# save only name of packages
rev `pwd`/TMP/32bit.rpm.lista0 | cut -d"-" -f 3- | rev > `pwd`/TMP/32bit.rpm.lista2
#==================}

#------- CREATING THE LIST 32 BIT PACKAGES ----
#======================================================}


#========================================{
# note
echo "Sections: $(cat `pwd`/TMP/sections)" > `pwd`/TMP/apt.log
echo "apt-get install -s \$(/TMP/32bit.rpm.lista2)" >> `pwd`/TMP/apt.log
echo "-------------------------" >> `pwd`/TMP/apt.log

echo "Sections: $(cat `pwd`/TMP/sections)" > `pwd`/TMP/distupgrade.log
echo "apt-get  -s dist-upgrade" >> `pwd`/TMP/distupgrade.log
echo "-------------------------" >> `pwd`/TMP/distupgrade.log
#========================================}


#==================================={
# 2. Save the simulation command to a file
echo " apt-get update ; \
apt-get install -s \$(cat \`pwd\`/TMP/32bit.rpm.lista2) |& tee -a \`pwd\`/TMP/apt.log ; \
apt-get  -s dist-upgrade |& tee -a \`pwd\`/TMP/distupgrade.log" > `pwd`/TMP/komenda
#===================================}


#====================={
# make file executable
chmod +x `pwd`/TMP/komenda

# 3. Run file with root
gksu -u root `pwd`/TMP/komenda
#====================={


#==================================={
# 4. Open log with default text editor
xdg-open `pwd`/TMP/apt.log
xdg-open `pwd`/TMP/distupgrade.log
#===================================}
echo "The End"

```


# **3. Comparison of update commands.**

Correct update system procedure is
```
apt-get update 
apt-get dist-upgrade
```

I tested 3 commands and synaptic with test section

**1. apt-get install -s $(/TMP/32bit.rpm.lista2)**
```
The following packages have unmet dependencies:
  dcraw-gimp2.0: Conflicts: ufraw
  libdc1394_12-devel: Conflicts: libdc1394-devel
  libraw1394-devel: Conflicts: libraw1394_8-devel
  libraw1394_8-utils: Conflicts: libraw1394-utils
  php-gd: Obsoletes: php-gd-bundled
  task-lamp: Obsoletes: php-sqlite3
  ufraw-gimp: Conflicts: dcraw-gimp2.0
E: Broken packages
```

**2. apt-get  -s upgrade**
```
...
The following packages have been kept back
   gtk+3.0 (3.14.14-2pclos2015 => 3.22.5-1uplos2016)
   libapr1_0 (1.4.6-1pclos2013 => 1.5.2-1uplos2016)
   libgtk+3_0 (3.14.14-2pclos2015 => 3.22.5-1uplos2016)
   libvte2_90_9 (0.36.5-1pclos2015 => 0.36.5-4uplos2016)
   mkvtoolnix (8.7.0-1pclos2016 => 9.3.1-1uplos2016)
   newt (0.52.18-1pclos2014 => 0.52.19-1uplos2016)
   phonon-vlc (0.8.2-4pclos2015 => 0.8.2-9uplos2016)
   simpleburn (1.6.5-1pclos2012 => 1.8.1-1uplos2016)
   vlc (2.2.2-1pclos2016 => 2.2.4-2uplos2016)
   vte3 (0.36.5-1pclos2015 => 0.46.0-1uplos2016)
   yajl (2.0.1-1pclos2012 => 2.0.4-1uplos2016)
   zcip (4-9pclos2010 => 4-10uplos2016)
169 upgraded, 0 newly installed, 0 removed and 12 not upgraded.
...
```

**3. apt-get  -s dist-upgrade**
```
...
The following packages will be REPLACED:
   gnome-icon-theme (3.14.0-1pclos2015) (by adwaita-icon-theme)
   gnome-icon-theme-symbolic (3.12.0-1pclos2014) (by adwaita-icon-theme)
The following packages will be REMOVED:
   libapr-util1_0 (3.14.0-1pclos2015)
   libserf1 (3.12.0-1pclos2014)
   libsvn0 (1.5.4-1uplos2016)
The following NEW packages will be installed:
   abattis-cantarell-fonts (0.0.24-1uplos2016)
   adwaita-icon-theme (3.22.0-2uplos2016)
   fontpackages-filesystem (1.44-1pclos2014)
   gnome-themes-standard (3.22.2-1uplos2016)
   lame (3.99.5-1pclos2013)
   libboost_filesystem1.60.0 (1.60.0-2uplos2016)
   libboost_regex1.60.0 (1.60.0-2uplos2016)
   libnet1.0.2 (1.0.2a-4pclos2007)
   libpcre2_0 (10.22-1uplos2016)
   libpython3.4 (3.4.3-4uplos2016)
   libqtx11extras5 (5.4.2-1pclos2015)
   libvte2.91_0 (0.46.0-1uplos2016)
   libx265_79 (1.9-1uplos2016)
   libyajl2 (2.0.4-1uplos2016)
   phonon (4.8.3-3pclos2016)
   python3 (3.4.3-4uplos2016)
   vte2.90 (0.36.5-4uplos2016)
   vte3-profile (0.46.0-1uplos2016)
181 upgraded, 18 newly installed, 2 replaced, 3 removed and 0 not upgraded.
...
Conf adwaita-icon-theme broken
 Obsoletes:gnome-icon-theme Obsoletes:gnome-icon-theme-symbolic  []
E: Conf Broken adwaita-icon-theme

```

**4. Synaptic**
```
To be removed
  gnome-icon-theme
  gnome-icon-theme-symbolic
  libapr-util1_0
  libserf1
  libsvn0
To be upgraded
  ...
```
