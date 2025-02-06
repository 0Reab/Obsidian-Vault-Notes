### **Final Evaluation** 

|Category|Score|Comments|
|---|---|---|
|**Linux Basics**|✅✅✅✅✅ (5/5)|Solid!|
|**Processes & Users**|✅✅✅ (3/5)|Weak on `nohup`, `nice`, `who`|
|**Networking**|✅✅✅✅ (4/5)|Improve `iptables` vs `firewalld`|
|**Security**|✅✅✅ (3/5)|Learn Fail2Ban, chroot jail, file capabilities|
|**Firewalling**|✅✅ (2/5)|Learn firewall rules|
|**Troubleshooting**|✅✅✅✅ (4/5)|Good understanding!|

**Total Score: 21/30**

Based on 30 questions for important linux stuff, here is what I was missing.

1. File system and Paths - what each path is for...
2. Remember cat command is often useless, grep 'string' filename.txt
3. Note - hard links point to inodes and soft links point to filename.
4. 'nohup command &' run command that is persistent thru logouts/hangups or terminal closing.
5. ss is alternative to nestat (newer and with more features).
6. learn about firewalld and iptables (firewalld is frontend for iptables/nftables).
7. setuid - runs a program as owner instead of the user executing it.
8. capabilities - getcap /bin/ping
9. journalctl - sys logs
10. auditd - detailed security logging
11. fail2ban - failed login attempt = login ban
12. /etc/nologin - prevents non root users from logging in (used for maintenance)
13. alternative auth methods for ssh - MFA, hardware keys.
14. AppArmor and SELinux