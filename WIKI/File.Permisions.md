I will show on the example :

I will create empty file for it.
```
# touch file
```


**How check file permissions**
<blockquote># ls -l file
-rw-r--r-- 1 guest guest 0 Sep 12 15:42 file</blockquote>


What does it mean ?
https://wiki.archlinux.org/index.php/File_permissions_and_attributes


How change permissions ?
For example change read, write, execute only for root ...
- Try use example chmod calculator on-line
example :
http://www.onlineconversion.com/html_chmod_calculator.htm
https://chmod-calculator.com/

Now you know, it will 700, so try change and check
<blockquote># chmod 700 file
# ls -l file
-rwx------ 1 guest guest 0 Sep 12 15:42 file*</blockquote>


How change owner of the file to root ?
<blockquote># chown root file
# ls -l file
-rwx------ 1 root guest 0 Sep 12 15:42 file*</blockquote>


How change group of the file to root ?
<blockquote># chgrp root file
# ls -l file
-rwx------ 1 root root 0 Sep 12 15:42 file*</blockquote>


**#** --> commands which I executed from root
$ --> commands which I executed from user ( I not used it :D )


Why this is it so important ?
Because operating systems use several security features, which you should know :
- how use firewall
- how change file permissions
and maybe something else ...


That means, from user account you should edit files only from home directory
/home/your_user_name/

Shortcuts to the same home folder :
$HOME
~/
