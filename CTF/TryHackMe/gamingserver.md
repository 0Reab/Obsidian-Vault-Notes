```shell
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)

/secret
/uploads
/secret? -- rsa_key found

http://ip/uploads/dict.lst

freewebsitetemplates.com
<!-- john, please add some actual content to the site! lorem ipsum is horrible to look at. -->

users: john

lessons learned :)
	after being a dumbfuck for about an hour almost, realising the ssh rsa private key i have is encrypted.
	and you can use it only if you know the pass phrase.
	use linpeas to enumerate for privilege escalation vectors.
	In this case you don't have sudo password for john, but xld is the only prvi esc vector left.


┌──(kali㉿kali)-[~/ctf/gamingserver]
└─$ while read -r psw; do openssl rsa -in secretKey -out key -passin pass:${psw} 2>/dev/null && echo "$psw"; done < dict.lst
letmein

and that's how it worked out at the end.

uname -a: Linux exploitable 4.15.0-76-generic #86-Ubuntu SMP Fri Jan 17 17:24:28 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

