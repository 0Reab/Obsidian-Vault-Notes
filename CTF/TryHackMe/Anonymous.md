```shell
ports:
	21/tcp  open  ftp         vsftpd 2.0.8 or later
	22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
	139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
	445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)

---------------------------------------------------------------------------------------------------------

smb free info:
	share - pics (might do some steganography)

---------------------------------------------------------------------------------------------------------
user:[namelessone, nobody
---------------------------------------------------------------------------------------------------------
info:
	Linux anonymous 4.15.0-99-generic #100-Ubuntu SMP Wed Apr 22 20:32:56 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux


namelessone@anonymous:~$ cat .wget-hsts
cat .wget-hsts
# HSTS 1.0 Known Hosts database for GNU Wget.
# Edit at your own risk.
# <hostname>    <port>  <incl. subdomains>      <created>       <max-age>
raw.githubusercontent.com       0       0       1589401240      31536000
github.com      0       1       1589401239      31536000

linpeas: red/yellow
	-rwsr-xr-x 1 root root 35K Jan 18  2018 /usr/bin/env ---> ended up using this as priv esc
	uid=1000(namelessone) gid=1000(namelessone) groups=1000(namelessone),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)
	Sudo version 1.8.21p2
	Ubuntu 18.04.4 LTS
	* * * * * /var/ftp/scripts/clean.sh

to do:
	ok - check ftp w anon
	ok - check ftp exploits - didnt work with anon only login enabled
	ok - check smb pics - found two images 
	ok - cve for smb on 139 3.x? - seems up to date, tried eternal blue didn't work non vulnerable
	no - ssh login after all else
	no - steganography brute force two images from smb pics share.
	ok - use bash script in anon ftp as a vector for shell on system - trying netcat revsh command and using ftps append command to send payload to bash script for deletion
		then let cron job run it, the line for shell is added at the bottom, ok using bash reverse shell, and put cmd from ftp, clean.sh will be overwritten with payload.

	no - Linux anonymous 4.15.0-99-generic find cve maby?
	no - wget works no need for this ---> Edit .wget-hsts in home dir of namelessone, to add my ipv4 as known to upload payloads. echo "10.9.163.154 8000" >> .wget-hsts

-----------------------------------------------------------------------------------------------------------
[-] 10.10.206.77:21 - This server is configured for anonymous only and the backdoor code cannot be reached
[*] Exploit completed, but no session was created.
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > 
-------------------------------------------------------
msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit
[*] Started reverse TCP handler on 10.9.163.154:4444 
[*] 10.10.206.77:139 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[-] 10.10.206.77:139      - Host does NOT appear vulnerable.


What I learned:
	Mr dum dum sent a revshell payload with wrong ip, then wondered why it no work, very nice, try that again :).
	* Pay attention, don't approach things with "i think'eth thy payload has correct ip" rather check and don't be lazy bum.
	* Be more thourough with researching red/yellow results from linpeas privesc results, super easy /usr/bin/env method was there but you gave it little attention.
		On the other hand trying to utilize the other red/yellow method via lxd was ok if it wasn't for bricked mirrors and and other issues with that repo/program.
	* Learn and understand new privesc method, on gtfobins if /usr/bin/env has suid bit set, the example cmd is just "./env /bin/sh -p" where you tried "/bin/sh -p"
		which didn't work, just downgraded your current shell.
		But using the path from the actual machine /usr/bin/env /bin/sh -p did escalate to root user.
		What the command does, uses /usr/bin/env to run /bin/sh which is a shell path and -p means run as privileged user, so in short we spawn a root shell via environment.
	* On the other hand finding that clean.sh script that is ran by cron job and understanding that from the log files, and utilizing it for the shell was very nice.


```