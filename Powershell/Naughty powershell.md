```powershell
powershell -WindowStyle hidden -executionpolicy bypass;
```

Obfuscation:
```shell
cd /home/kali/fun/pwrshll # my custom own path
pwsh
Import-Module ./Invoke-Obfuscation/
Invoke-Obfuscation
SET SCRIPTPATH <path to your script>
TOKEN
ALL
1
```

```powershell
`Start-Process powershell -ArgumentList "-NoProfile -WindowStyle Hidden -Command {Your-PowerShell-Command}" -PassThru`
```