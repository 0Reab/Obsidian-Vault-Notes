Short guide on how to crack a hashed password.

```sh
hash-identifier # run this
# in prompt enter the hashed psw

john --list=formats | grep 'md5' # find the format name

john --format=md5crypt --wordlist=${ROCKYOU} hash.txt
# wordlist is a rockyou path and the hash is in the .txt file
# path /usr/share/wordlists/rockyou.txt
# hash $apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.
```