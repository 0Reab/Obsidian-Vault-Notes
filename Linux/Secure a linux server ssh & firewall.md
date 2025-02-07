In order to have a hardened linux server, we need to make sure we can access the machine and harden it.
We can do this via ssh.

Upon ssh-ing into the server here are some things we should do.
- Update the system and enable automatic updates.
- Configure the ssh to use public and private key instead of using the password.
- Disable root login.

Next for networking:
- We need to check open ports via 
```shell
ss -tulnp
```
- Get a ufw (uncomplicated firewall - frontend cli)
- Close unnecessary ports.
- Change ssh default port to some uncommon port number.
- Disable ICMP ping responses (if you ping the server your requests will time out)