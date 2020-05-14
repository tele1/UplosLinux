1. The basic method of checking whether our package needs to install something,
is to install the package on a minimalist system.
When running app from terminal and you see errors, warnings about missing libs, app not working correctly
 and you probably need dependencies.

2. Useful diagnostic and debugging tools,
**ldd**  -->   prints the shared libraries required by each program or shared library
**strace**  -->  trace system calls and signals when app running
More in:
About debug:  http://pclinuxoshelp.com/index.php/Debug_a_Program
About ldd   http://pclinuxoshelp.com/index.php/Ldd

3. About rpm.
When you build package, rpm reads and executes commands from the .spec file
Most of these commands these are macros,
for example %configure and %cmake.
At the end rpm uses  find-requires script to find dependencies.
you have it printed almost at the end of the rpm build log.
script use ldd command to find part dependencies ( http://www.rpm.org/max-rpm/s1-rpm-depend-auto-depend.html )
But that are not all dependencies and rpm with ldd tool can find only files (not packages),
 and it can not find the needed packages from repository to install.

```
$ locate  find-requires | grep -v "home"
/usr/bin/mono-find-requires
/usr/lib/rpm/find-requires
/usr/lib/rpm/mono-find-requires
/usr/lib/rpm/ocaml-find-requires.sh
/usr/lib/rpm/mandriva/find-requires
```

How should work rpm may be more problematic.
 "ldd" can find dependencies after compiled files,
we can also build script to find python dependencies. ( https://docs.python.org/3/tutorial/modules.html )
But we have also qml scripts, here dependencies should be found before compile source code ( http://doc.qt.io/qt-5/qmlmodules.html )
and perl modules ( http://www.symkat.com/find-a-perl-modules-path )
and we have also configure and cmake scripts attached to the source code
 which can provide own, sometimes outdated dependencies.

Debian have own scipts, for me is interesting tool which can 
read dependencies from configure script,
how it work  http://stackoverflow.com/questions/997117/how-to-make-configure-script-check-the-dependencies 

I created some scripts, but use is not comfortable, because you need to know what's for what,
and it does not take into account all, for example python scripts.
Embedding it in the rpm can be problematic , if we need find for example find cmake and configure commands.
Maybe rebuild and placement in macros will better way, I don't know.
  https://github.com/tele1/PackagerScripts
