```powershell
$PSVersionTable.PSVersion # powershell version
Write-Host "hello world" -NoNewline # echo
# comment

Get-Command -CommandType Cmdlet # find cmdlets
Get-Help [cmdlet] -Detailed -Full # man pages / help pages
Write-Host "write in file" | Out-File out.txt

$FavCharacter = "Sponggebob" # variable
$FavCharacter | Out-File char.txt # file write
cat .\char.txt # output file contents

$FavCharacter.GetType() # returns type (float, int32, str, double, bool)

$FavCharacter.Length # len()
$FavCharacter | Select-Object -Property * # retun (all) properties of object $Fav...
Get-Member -InputObject $FavCharacter # method list applicable to object

$Jedi = @('Obi-Wan Kenobi', 'Yoda') # array
$Jedi[0] # array return by index
$Jedi[1].Length # 4 - method called upon array element
$Jedi += "new element" # append to array

# hashtables
$Fellowship = @{"wizard" = "Gandalf"; "hobbit" = "Frodo"} # init
$Fellowship.Add("dwarf", "Gimli") # add
$Fellowship.Set_Item("dwarf", "Bilbo") # update
$Fellowship."dwarf" # call
$Fellowhsip["dwarf"] # call
$Fellowship.Remove("dwarf") # remove

Set-Alias vi nvim # alias a command
Set-Alias -Name nvim -Value C:\....\nvim.exe # alias bin

Write-Host # writes to console
Write-Output # writes to pipeline - is implicitly called - Get-Service | Write-Output

$FavOs = Read-Host -Prompt "What is your favorite OS?"
```

```powershell
$PokemonCaught = "908"

if ($PokemonCaught -eq 908) {
    Write-Host "You are a master!"
}
Else {
    Write-Host "Catch more!"
}
```

```powershell
$PokemonNum = 3253

If ($PokemonNum -ge 1 -and $PokemonNum -le 151) {
    Write-Host "Your Pokemon is from Kanto!"
}
Elseif ($PokemonNum -ge 152 -and $PokemonNum -le 251) {
    Write-Host "Your Pokemon is from Johto!"
}
Elseif ($PokemonNum -ge 252 -and $PokemonNum -le 386) {
    Write-Host "Your Pokemon is from Hoenn!"
}
```

```powershell
$House = "Targaryen"

Switch($house) {
    "Targaryen" { Write-Host "You are crazy!"; break }
    "Lannister" { Write-Host "You always pay your debts!"; break }
    "Stark" { Write-Host "The wall incident!"; break }
}
```

```powershell
$HaloPeeps = $('Master Chief', 'Cortana', 'Flood')

For($counter=0; $counter -le ($HaloPeeps.Lenght - 1); $counter++) {
    Write-Host "Holy... it's" $HaloPeeps[$counter]
}
```

```powershell
$HaloPeeps = $('Master Chief', 'Cortana', 'Flood')

Foreach ($peep in $HaloPeeps) {
    Write-Host $peep
}
```

```powershell
$Xmen = @('Wolverine', 'Cyclops', 'Storm')
$counter = 0

While ($counter -lt $Xmen.Length) {
    Write-Host $Xmen[$counter]
    $counter ++
}
```

```powershell
$Xmen = @('Wolverine', 'Cyclops', 'Storm')
$counter = 0

Do {
    Write-Host $Xmen[$counter] "is a mutant!"
    $counter++
} while ($counter -ne $Xmen.Length)
```

```powershell
function Test-SpaceX {
    [CmdletBinding()] # turns into adv. function
    param(
        [Parameter(Mandatory)]
        [int32]$PingCount
    )

    Test-Connection spacex.com -Count $PingCount
}

Test-SpaceX -PingCount 10
```

```powershell
Throw "It's a trap!" # throw exception
Write-Error -Message "It's a trap?" -ErrorAction Stop # throw error?
```

```powershell
# error handling
try { Test-SpaceX -ErrorAction Stop } catch { Write-Output "Launch problem!" Write-Output $_ }
```

```powershell
# create file
New-Item -path C:\Scripts -name "something" -type "file" -value "xd"
```

notes:
- use Get-Help to find syntax for cmdlet and it's set aliases
- tree /f - tree with files
- ls -Name - only filenames