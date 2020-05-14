Dex-autostart
This package is a facility to do a startup application. For ex. @threepio asked me to do a desktop entry for mate-sensors-applet directly from installation, but I think that everyone can decide which applet to install (or not) in mate DE. A help for create a desktop entry I can give you with dex-autostart. Download and install it from synaptic. Then open a terminal and, for the example above, type dex-autostart -t ~/.config/autostart -c /usr/lib/mate-sensors-applet/mate-sensors-applet
-t is where file.desktop will be create and -c is where is the executable. For other commands this is man-page https://github.com/jceb/dex
WARNING our executable is dex-autostart 
