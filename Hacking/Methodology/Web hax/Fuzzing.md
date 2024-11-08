```shell
ffuf -request filehttpreq.txt -reqproto http -w ~/pathtowordlist.txt:FUZZ
# FUZZ - replacing this word with wordlist entries
```