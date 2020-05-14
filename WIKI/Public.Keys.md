**FOR GPG**

**How to check a list of keys ?**
- Public keys
```
gpg -k
```

- Only own secret keys
```
gpg -K
```


**How create own key ?**
https://www.gnupg.org/gph/en/manual/c14.html

Tips:
<blockquote>gpg --gen-key ...
We need to generate a lot of random bytes. ... </blockquote>
- Move cursor to create unique key, many times move ... .

More in **gpg --help** 
______________________________________________


**FOR RPM**

**For what exist signing packages ?**
- When package is sign, with gpg public key you can verify who created package.
If public key is suspicious, you can always verify key with creator key.

*Personally, I am not convinced about the effectiveness of the method in rpm ( now ).
Because you can always copy a key from one package and paste it in another.
The best way we should publish the checksums of files, on another known server.
Benefits of checksums:
- you can not change file without change checksum, because verification will incorrect
- you can not change checksum if someone can check, verify
If the checksum inside rpm is built from a file and a key, this is only better way and nothing more.
The keys are used for encryption. And they work perfectly there.
Remember only use the strongest checksums as much as possible ( read about collision in checksums )*
http://www.liberainformatica.it/forum/showthread.php?tid=719


**How import public keys to rpm?**
```
rpm --import RPM-GPG-KEY-name
```
RPM-GPG-KEY-name --> this is file


**How  verify rpm package ?**
```
rpm -K name_package
```

- Example ( this command will check md5 and gpg key )
```
rpm -K uplos-icon-theme-2-1uplos.noarch.rpm
uplos-icon-theme-2-1uplos.noarch.rpm: rsa sha1 (md5) pgp md5 OK
```
here existence "** pgp **" and "** OK **" at the end it is very important.
"** pgp **" --> package have key
"** OK **" --> all is ok.


**How create own key ?**
https://gist.github.com/fernandoaleman/1376720
http://www.liberainformatica.it/forum/showthread.php?tid=691


**How to check a list of keys ?**
- Public keys used by rpm
```
rpm -qa gpg-pubkey* 
```
```
 rpm -q gpg-pubkey --qf '%{NAME}-%{VERSION}-%{RELEASE}\t%{SUMMARY}\n' 
```



**About signing an RPM with a GPG key**
http://cholla.mmto.org/computers/linux/rpm/signing.html
[hr]


**About  Encrypt**

1. Encrypt files with password
GnuPG  https://www.cyberciti.biz/tips/linux-how-to-encrypt-and-decrypt-files-with-a-password.html
OpenSSL  https://rietta.com/blog/2012/01/09/openssl-encrypt-file-with-password-from/
Other way with OpenSSL  https://www.shellhacks.com/encrypt-decrypt-file-password-openssl/

2. Why symmetric encryption is not safe to send files
https://www.youtube.com/watch?v=CR8ZFRVmQLg

3. Asymmetric encryption with 2 keys: private and public
https://www.youtube.com/watch?v=E5FEqGYLL0o

 - GPG  https://www.gnupg.org/gph/en/manual/x110.html
 - OpenSSL http://krisjordan.com/essays/encrypting-with-rsa-key-pairs

4. Other use of the gpg:
**- File encryption with password. ( asymmetric encryption )**

**Encryption:**
```
gpg -c file
```
When we want create binary file. ( file.**gpg** )
( Here we give new a password to encrypt )

```
gpg -c -a file
```
When we want create text file, for example for send email. ( file.**asc** )
( Here we give new a password to encrypt )

**Decryption:**
```
 gpg -d file.gpg > file.decoded
```
or
```
 gpg -d file.asc > file.decoded
```
( We give a password created before to decode )
Then content decoded file will be saved to file.decoded

_____________________________________________

**- File encryption with public key.**

**Encryption:**
```
$ gpg -e file 
You did not specify a user ID. (you may use "-r")

Current recipients:

Enter the user ID.  End with an empty line:
```
write User Id ( name or email ... ), ... enter
enter.
And now you have encrypted file  **file.gpg** ( binary file )

**Decryption:**
Sent file ( file.gpg ) , then our friend can use own key ( private key ) 
to decode file.
```
$ gpg -d file.gpg 

You need a passphrase to unlock the secret key for 
... 
```
Write passphrase, then  you will see content file.


Edited 29.10.2018
- Two guides have been combined.
- Tutorial was moved here from devel section.
