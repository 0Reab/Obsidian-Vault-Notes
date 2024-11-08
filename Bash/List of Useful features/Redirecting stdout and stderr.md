When running a command like ping for example.
The command might produce unwanted verbose output in your terminal.

Examples:
```bash
ip=192.168.1.11
log="ping_log.txt"

ping -c 4 "$ip" 1>"$log"   # redirect standard output
ping -c 4 "$ip" 2>"$log"   # redirect standard error
ping -c 4 "$ip" &>"$log"   # redirect both stdout and stderr
```

Additionally if you want to discard either stdout or stderr you can redirect it to /dev/null 
```bash
ping -c 4 "$ip" &>/dev/null
```