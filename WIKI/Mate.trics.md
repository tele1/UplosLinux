

at-spi-registry prevents to power-off or reboot on mate. You can do, as root,
mv /usr/lib/at-spi2-registryd /usr/lib/at-spi2-registryd.old
and
mv /usr/lib/at-spi-bus-launcher /usr/lib/at-spi-bus-launcher.old
Here it is the bug https://bugs.launchpad.net/ubuntu/+source/unity/+bug/729827 and workaround is to #18 
