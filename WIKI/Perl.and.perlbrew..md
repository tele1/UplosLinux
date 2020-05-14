So I'm still here to talk about perl. I was able to rebuild last 5.22.3 perl from pclos repo, but if I try to install it all system goes away, because many dependencies will break, even kernel! So how I'll resolve? Perlbrew is a matter to install multiple perl version in your home. To install perlbrew open a terminal and type:
$ curl -L https://install.perlbrew.pl -o install.perlbrew.pl
$ cat install.perlbrew.pl | bash
$ ~/perl5/perlbrew/bin/perlbrew self-install
Perlbrew is now installed, but you'll want to modify your shell's configuration files to make it easier to use. Open the file ~/.bash_profile in your editor, I used pluma and added the following line at the bottom of the file to include the Perlbrew settings:
source ~/perl5/perlbrew/etc/bashrc
Save the file and exit the editor. Then log out and then log back in to ensure that your .bash_profile file loads Perlbrew's settings. It will now add /home/youruser/perl5/perlbrew/bin to the front of your PATH environment variable, and set some other environment variables Perlbrew needs.
Verify that these environment variables have been set by running the env command and filtering the results with grep for the text PERL:
$ env | grep PERL
Let's use Perlbrew to install a stable version of Perl 5. Use the perlbrew command to see which Perl versions are available for installation:
$ perlbrew available
Install a version with:
$ perlbrew install perl-5.xx.x change xx.x with yours choiched version
The Perl installation can take quite a while to build and install. Do not interrupt the build process. If you want to see the build's progress, you can open a separate terminal session and monitor the build log with
$ tail -f ~/perl5/perlbrew/build.perl-5.xx.x.log
To use your new Perl installation, run the following command:
$ perlbrew use perl-5.xx.x
To use perl module if you need for your version
$ curl -L https://cpanmin.us | perl - App::cpanminus to install for ex.:
$ cpanm Email::Simple
