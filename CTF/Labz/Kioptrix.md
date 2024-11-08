mod_ssl/2.8.4 - mod_ssl 2.8.7 and lower are vulnerable to a remote buffer overflow which may allow a remote shell. - (nikto scan results)

Apache httpd 1.3.20

SMB: Samba 2.2.1a
msf > use exploit/linux/samba/trans2open

└─$ smbclient -L \\\\10.0.2.15\\ 

IPC$            IPC       IPC Service (Samba Server)
ADMIN$          IPC       IPC Service (Samba Server)
└─$ smbclient \\\\10.0.2.15\\IPC$ or ADMIN$

SSH: OpenSSH 2.9p2 (CVE-2023-38408)
RCE vulnerability (score: bing bong)

ssh 10.0.2.15 -oKexAlgorithms=+diffie-hellman-group1-sha1 -oHostKeyAlgorithms=+ssh-dss -c aes128-cbc (establish a connection >.<

Webalizer Version 2.01 (CVE-2002-0180)
XSS vulnerability (score: ~7)


