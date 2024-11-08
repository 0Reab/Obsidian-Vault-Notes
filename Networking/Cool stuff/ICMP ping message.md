```shell
└─$ ping -c1 -s32 -p$(echo 'helloworld' | xxd -p) 8.8.8.8

```
This is how you can ping a server once using 32 bytes to send an string as hex dump.