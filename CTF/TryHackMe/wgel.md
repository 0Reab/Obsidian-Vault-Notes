CMS --- unapp --- jquery, images/cover_img_1.jpg (source code path info leak), colorlib template

paths:
	/sitemap

ports:
	22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
	80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))


inodes accesible to users.

------------------
---same mistake again, tunnel vision, do better enumeration, mistake of assuming one worlist would be enough for directories.---
---dont forget you are doing a ctf check stupid things---
---good point on understaning gobuster is non recursive, and running a scan on /sitemap---
------------------

legit after testing lots of stuff and no results.
on default apaches page there is a comment about user jessie ... ctf moment right there

Ok so user jessie, on thing i learned is that wordlist medium 2.3 might be large but it doesn't contain what common.txt wl contains.
That being entries such as .ssh, as you might assume by now ther is a id_rsa private ssh key on this server that is accessible.
Tried briefly bruteforcing jessies ssh, nothing, scanning nmap ports > 1000 unlikely I would find anyhting, but I was desparate.

Okay sice we have the id_rsa, you utilze it as such:
┌──(kali㉿kali)-[~/ctf/wgel]
└─$ sudo chmod 600 id_rsa  # 600 sets the file so only the user who owns it can read write or execute the file aka rwx, set file to ony user rwx 

┌──(kali㉿kali)-[~/ctf/wgel]
└─$ sudo ssh -i id_rsa jessie@10.10.10.10

jessie@CorpOne:/$ sudo -l
User jessie may run the following commands on CorpOne:
    (ALL : ALL) ALL
    (root) NOPASSWD: /usr/bin/wget

Okay the idea was that since gtfo bins has some priv esc methods using wget, they don't work beacuse the option --use-askpass doesnt exist in this version or it's a patched wget.
So you can use wget to exfiltrate files in root dir, you will need an http server that will work with post reqs so reqular http.server from python wont do, it's easy dont worry.
And once that is sorted utilize gtfo bins to download the flag in root directory, I gave up to early the flag is /root/root_flag.txt 
The find command didn't work out for finding the flag so maby autoamting this task for finding the flag may have been possible if jessie can make bash / py scripts.
in any case the idea was there.
