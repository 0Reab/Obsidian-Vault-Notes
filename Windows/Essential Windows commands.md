Used for system admins and in cyber security.

[[ipconfig]] /all 
[[netstat]] -ano, -ab
[[nslookup]] google.com
[[tracert]] 168.126.x.x 
[[arp]] -a 
[[systeminfo]] 
[[tasklist]] /m 
[[taskkill]] /IM "id" /F 
[[sfc]] /scannow 
[[chkdsk]]
[[gpupdate]]
gpresult /force
[[gpedit.msc]]
[[netuser]] username password /add 
[[regedit]]
[[gpedit.msc]]  
net group groupname /add
[[shutdown]] /s /t 0
[[shutdown]] /r /t 0
[[shutdown]] /r /fw (to bios)
[[netsh]] interface show interface 
[[sc]] query | find "xy" 

lil advanced...

wmic os get caption, version
powercfg query
bcdedit
diskpart 
schtasks
robocopy
wevutil
dism
reg

# Naughty commands...
cipher /w:C:\
cipher /e /s:C:\myfolder