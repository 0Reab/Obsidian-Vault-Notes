This method of priv escal in linux works if the user has sudo privilages to run command 'less'.
less is an program useful for reading large text files because it loads quickly page by page.

Exploiting this can be do by spawning a root shell using less command.

Example:
```shell
touch file.txt
sudo less file.txt
```

And when inside the less program, pressing ':' will allow you to interact with the program and type !/bin/sh to spawn the shell.

If the command less is accessible but without sudo privileges, it can still be utilized to possibly edit files etc...

https://gtfobins.github.io/gtfobins/less/