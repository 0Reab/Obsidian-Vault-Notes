In this note we will be exploring how to utilize windows features for hacking AD.
So not misconfigurations, or using credentials.

### LLMNR Poisoning

`linked local multicast name resolution`

- It resolves domains when dns is down.
- Previously known as NBT-NS (netbios name service)
- Key flas is that the services utilize a user's username and NTLMv2 hash when appropriately responded to.

In this diagram we have a man in the middle attack where the user for example type in something wrong which is causing a dns issue.

![[Pasted image 20241125160755.png]]

So the victim machine does a broadcast message, where we come in as an attacker.
For this scenario we can use the tool `responder`.

![[Pasted image 20241125161024.png]]

It's a good idea to run this thing first, because essentially we need loads of traffic to eventually capture that request for resolution.

![[Pasted image 20241125161256.png]]

![[Pasted image 20241125161328.png]]

If the passwords are weak, they will be cracked with tools like hashcat and a nice wordlist like rockyou.txt
