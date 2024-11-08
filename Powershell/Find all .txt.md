```powershell
Get-ChildItem -Path C:\ -Filter *.txt -Recurse -File
```

```powershell
Start-Process powershell -ArgumentList "-NoProfile -ExecutionPolicy Bypass -File `"$PSCommandPath`"" -Verb RunAs

2>$null
```
