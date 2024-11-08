```powershell
powershell -WindowStyle hidden -executionpolicy bypass;
```

Obfuscation:
```shell
pwsh
Import-Module ./Invoke-Obfuscation/
Invoke-Obfuscation
SET SCRIPTPATH <path to your script>
TOKEN
ALL
1
```