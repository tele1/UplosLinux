First of all we have java jre but not java jdk. This last is Java Development Kit, it has everything the JRE has, but also the compiler and tools, it can create and compile java programs. You go to this link
https://www.oracle.com/java/technologies/jdk8-downloads.html
Accept license and download .rpm file. Next you can install it with terminal or, more easy, with rpm-installer.
Now we install last available Maven
$ wget https://www-us.apache.org/dist/maven/mav...bin.tar.gz -P /tmp
Extract the archive in /opt
$ su + your root password
# tar xf /tmp/apache-maven-* .tar.gz -C /opt
Create a symbolic link
# ln -s /opt/apache-maven-3.6.0 /opt/maven
Now setup environment variables
# nano /etc/profile.d/maven.sh
Paste the following configuration

export JAVA_HOME=/usr/java/default
export M2_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}

Save (ctrl+o) and exit (ctrl+x)
Make sure that script is executable
# chmod +x /etc/profile.d/maven.sh
Finally we load the environment with
# exit
$ source /etc/profile.d/maven.sh
We can verify good installation using mvn -version
$ mvn -version
You will see somethink like this:
Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-24T20:41:47+02:00)
Maven home: /opt/maven
Java version: 1.8.0_202, vendor: Oracle Corporation, runtime: /usr/java/jdk1.8.0_202-i586/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.1.13-pclos1", arch: "i386", family: "unix"

Next we will find what maven does
