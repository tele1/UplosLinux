I will not write a detailed guide, but I will write how to get started.

**1. Basic materials to read:**
https://developer.gnome.org/integration-guide/stable/icons.html.en
https://specifications.freedesktop.org/


2. Where find example icons
Check **gnome-icon-theme** package
```
rpm -ql gnome-icon-theme
```


3. In brief:
Folder with gnome icons is in **/usr/share/icons/**
and have 
**8x8/  16x16/  22x22/  24x24/   32x32/   48x48/  256x256/  scalable/**  folders,
with 
**actions/     apps/        devices/  emotes/     places/
animations/  categories/  emblems/  mimetypes/  status/**  
folders with **.png** icons, except scalable/ folder which have **.svg** icons
and  **index.theme** file
and  **icon-theme.cache**  sometimes.

I'm guessing **.png** was popular before **.svg**.
For me .svg icons are better because this is vector graphics  https://en.wikipedia.org/wiki/Scalable_Vector_Graphics
So maybe in future we will use only .svg icons.
For example if not exist  22x22/ icons, are used scalable icons, so other are not needed (in theory)
However for now you can create .png  and .svg icons.

I suggest build first scalable icons:
```
scalable
├── actions
├── apps
├── categories
├── devices
├── emblems
├── emotes
├── mimetypes
├── places
└── status
```
then you can build similar icons in 16x16/  22x22/  24x24/   32x32/   48x48/  256x256/
with script.
For build scalable in .svg icons you can use **Inkscape**.

Example script 
for build 16x16/  22x22/  24x24/  ...  icons from scalable/
with uplos logo and two symbolic links

```
#!/bin/bash

# loop, 
for i in 16 22 24 32 48 ; do

	# create folders  
	install -d -m 0755 ${i}x${i}/places

	# create pictures .png
	inkscape -z scalable/places/uplos.svg -w $i -e ${i}x${i}/places/uplos_logo.png ;

	# create symbolic links
	ln -s uplos_logo.png ${i}x${i}/places/distributor-logo.png
	ln -s uplos_logo.png ${i}x${i}/places/start-here.png
done

#fix permissions
find . -type d -print0 | xargs -0 chmod 0755
find . -type f -print0 | xargs -0 chmod 0644
```

- Install **inkscape** before run script if you want use.
- Run script from the same place where is your scalable/ folder.

Then you can create **index.theme** file.
File should contain all the folders which we created for icons.
In file you can also find
```
Inherits=gnome
```

if you replace to
```
Inherits=my_icons,gnome
```
this / it , first icon will be loaded from " **my_icons** " icons then
if icon not exist, system will try load icon from " **gnome** " icons.

Now if you have ready icons
```
$ tree -L 2
.
└── my_icons
    ├── 16x16
    ├── 22x22
    ├── 24x24
    ├── 32x32
    ├── 48x48
    ├── index.theme
    └── scalable

```
You can install to
**/usr/share/icons/**  for all users
or to
**~/.icons/**  for only you in home directory.

Ready, now you can choose icons in system and test.


If you want just test icons other people,
you can search ready icons in
- https://store.kde.org/
- https://www.gnome-look.org/browse/ord/latest/
- https://www.xfce-look.org/browse/ord/latest/
