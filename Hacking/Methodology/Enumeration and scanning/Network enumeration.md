```shell
nmap -A -p- -sS $ip

```
-A : Full scan run nmap scripts like service detection and os detection i think
-p- : Scan all ports.
-Ss : Syn tcp scan.