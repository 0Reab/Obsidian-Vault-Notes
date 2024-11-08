Is used for expanding attack surface.

There are three methods for subdomain enumeration:
- Brute force
-  OSINT
-  Virtual Host

## OSINT SSL/TLS Certificates

When and SSL/TLS (Secure Sockets Layer/Transport Layer Security) certificate is created for a domain by a CA (Certificate Authority).
CA's create logs when doing so, and they are publicly accessible.
The purpose of the logs is to stop malicious and accidental certificates from being used.

Using this service for our advantage to discover subdomains using sites such as:
- https://crt.sh
- https://ui.ctsearch.entrust.com/ui/ctsearchui

(both current and historical certificates)

## OSINT Search Engines

Can be excellent resource for discovering subdomains.
Google dorking methods such as site:www.domain.com or site:*.domain.com 

Another example site:*.tryhackme.com -site:www.tryhackme.com

## DNS Bruteforce

Is a method of trying lots of possible subdomains from a pre-defined list of commonly used subdomains.
We can automate this process with a tool *dnsrecon*.

## OSINT Sublist3r

To speed up the process of OSINT subdomain discovery we can automate the above methods with tools such as sublist3r.

## Virtual Hosts

Some subdomains aren't always hosted in publically accessible DNS results, such as development versions of a web application or administration portals. Instead, the DNS record could be kept on a private DNS server or recorded on the developer's machines in their /etc/hosts file (or c:\windows\system32\drivers\etc\hosts file for Windows users) which maps domain names to IP addresses. 

Because web servers can host multiple websites from one server when a website is requested from a client, the server knows which website the client wants from the **Host** header. We can utilise this host header by making changes to it and monitoring the response to see if we've discovered a new website.

Like with DNS Bruteforce, we can automate this process by using a wordlist of commonly used subdomains.

Start an AttackBox and then try the following command against the Acme IT Support machine to try and discover a new subdomain.
```shell
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.145.23 -fs {size}
```