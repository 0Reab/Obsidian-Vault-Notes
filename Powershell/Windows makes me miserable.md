```powershell
# to set an fucking alias

Set-ExecutionPolicy RemoteSigned -Scope CurrentUser # allows user for running signed scripts
# or if you are spicy use Unrestricted for running any scirpts
# to be able to run $PROFILE configuration for persistant alias

vim $PROFILE
# use notepad also, set lua alias
Set-Alias lua lua54



```