```shell
while read -r psw; do openssl rsa -in secretKey -out key -passin pass:${psw} 2>/dev/null && echo "$psw"; done < dict.lst
```