Capturing the exit code of the command that ran last is useful for a script.
Exit code of 0 means the command executed as intended.
Any other exit code could mean many things mostly errors or unexpected outcome.

Checking the exit code is done via $?

```bash
ip_address=192.168.1.11

ping -c 4 $ip_address

if [ $? -eq 1 ]; then
	echo "Ping command failed."
else
	echo "Ping succesfull."
fi
```
In this example we ping an ip address 4 times, and check if the exit code is equal to 1.