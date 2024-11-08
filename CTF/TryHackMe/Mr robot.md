replace gowitness tool plz

all you need is a code?

apache http server
port 80 and 443, ssh 22 closed

post request for entering email on /join command aka dir
with value 'help'

wordpress website, has admin login panel visible


test:
        sql, xss, abuse http reqs, and maby try to pwn /join for email request (probably not good idea)
        metasploit search for enumeration, find a version? and look for xploits
        bruteforce admin login

tested sql injections, nothing, ~/Downloads/Generic_ErrorBased.txt:USERFUZZ -w ~/Downloads/Generic_ErrorBased.txt:PASSFUZZ
tested xss nothing /resetpsw



exploits: admin shell upload if you have admin creds

WordPress version 4.3.1 released on 2015-09-15
RCE rev shell via requires admin login - https://www.exploit-db.com/exploits/50255


ctf tasks find 3 keys.
1. in robots.txt there is a file key1out-3.txt so you can curl that file 073403c8a58a1f80d943455fb30724b9 curl <ip>/file.txt
        ther is also a fscoity.txt file maby use as wordlist as it hints for key 2? 'fishy' wordlist?
        will use fsocity.dic wordlist but need to find username to use, bruteforcing reset psw using http request and fuzzing with seclist huge usernamelist
        omg its elliot as username.
        └─$ cat fsocity.dic | sort | uniq > fsocity_uniq.dic (hint makes sense you can cut the wordlist by 1/10 of its size
        i guess not bruh i aint running allat psw is ER28-0652
        archive.php file edit with rev shell and access via ip/wp-content/twentyfifteen/archive.php 2015 is a plugin or theme and idk how you get to this url for archive i guess diggin
2. 
        $ cat password.raw-md5
        robot:c3fcd3d76192e4007dfb496cca67e13b
        /usr/bin/script -qc /bin/bash /dev/null === upgrade the shell :D
        md5 cracked psw -- abcdefghijklmnopqrstuvwxyz 
        key -- 822c73956184f694993bede3eb39f959


Thuings to learn:
        - How do you find where is archive.php path to run the reverse shell.
        - How does a basic .php reverse shell work?:
                - What is Daemonizing, zombie, and other new terms for the php revshell.
        - How does /usr/bin/script -qc /bin/bash /dev/null do to upgrade the shell?
        - Was it a mistake to use ffuf for psw login bruteforce or shoulda I use hydra next time?
        - Remember to look at robots.txt next time at the beginning.
        - Fix yo gowitness aka replace it.
        - Why is upgraded shell allowing for passw entry for sudo but it enters almost instantly? ok so non interactive like basic shells do not allow for runnin su, use python to
                make it interactive - python -c 'import pty;pty.spawn("/bin/bash")'
                ofc this upgraded shell works for entering password but other one doesnt? what the feck
        - hash-identifier tool kinda neat
        - finding suid binaries (bins with suid bit set for root) - fin / -perm +6000 2>/dev/null | grep '/bin/' = /usr/local/bin/nmap and we can run that bin with --interactive
                you can find this for any bin on gtfobins el classico
        - apparently I had the right ide with shortening wordlist of passwords see what I did wrong comparing with this comment:

One thing that was very crucial in brute forcing the password for Elliot is that there is nearly a 1,000,000 words in the the fsociety.dic when in reality it should have been about 11,000 words. If you cat fsociety.dic | grep "any word in the fsociety file here", you will see a ton of the same words being used in the file. To remove all those unnecessary duplicate words, you could have done sort fsociety.dic | uniq -d > new.txt. Then right after that you would append the unique words doing sort fsociety.dic | uniq -u >> new.txt. In doing so would give you the actual amount of words for that wordlist. That would save you a ton of time finding the password in a shorter time
