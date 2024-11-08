If you find users ssh private key (id_rsa), not to be mistaken with one that ends with .pub for public.
You can use that file to login in the ssh as that user if you supply username and the key.

Good practise is to have the private key with permissions where only you can have read permissions to it and nothing a nonone else can access/read/execute it.

I think these are the right perms 
```shell
chmod 600 id_rsa
```
Confirmation, 600 makes it readable and writeable only for owner aka you in this case.

now to use the key for login.
```shell
ssh -i id_rsa user@ip_addr
```