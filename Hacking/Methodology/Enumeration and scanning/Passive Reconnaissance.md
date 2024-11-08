-  whois - query WHOIS servers
-  nslookup query DNS servers
-  dig to query DNS servers

These are all public services available for passive recon, meaning we can gather information without interacting with the target and remain stealthy.

Additionally two more online services:
-  DNSDumpster
-  Shodan.io 

## Whois

Is a request and response protocol.
A whois server listens on port TCP 43 for requests.

Here are some things that we can learn about our target.
-  Registrar - Via which registrar was the domain name registered?
-  Contact info: Name, org, address, phone, etc... (unless made hidden)
-  Creation, update, expiration dates - regarding the domain
-  Name server: Which server to ask to resolve the domain name

You can use online tools or your command line to get whois data.
```shell
whois tryhackme.com
```
#### example result

```sh
user@TryHackMe$ whois tryhackme.com
[Querying whois.verisign-grs.com] 
[Redirected to whois.namecheap.com] 
[Querying whois.namecheap.com] 
[whois.namecheap.com]
Domain name: tryhackme.com 
Registry Domain ID: 2282723194_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.namecheap.com 
Registrar URL: http://www.namecheap.com 
Updated Date: 2021-05-01T19:43:23.31Z 
Creation Date: 2018-07-05T19:46:15.00Z 
Registrar Registration Expiration Date: 2027-07-05T19:46:15.00Z 
Registrar: NAMECHEAP INC 
Registrar IANA ID: 1068 
Registrar Abuse Contact Email: abuse@namecheap.com 
Registrar Abuse Contact Phone: +1.6613102107 
Reseller: NAMECHEAP INC Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited Registry Registrant ID: Registrant Name: Withheld for Privacy Purposes Registrant Organization: Privacy service provided by Withheld for Privacy ehf [...] URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/ >>> Last update of WHOIS database: 2021-08-25T14:58:29.57Z <<< For more information on Whois status codes, please visit https://icann.org/epp
```

-------------------------------------------------
## nslookup and dig

Using the nslookup tool we can specify a query type.
```sh
nslookup -type=A tryhackme.com
```

| Query type | Result             |
| ---------- | ------------------ |
| A          | IPv4 Addresses     |
| AAAA       | IPv6 Addresses     |
| CNAME      | Canonical Name     |
| MX         | Mail Servers       |
| SOA        | Start of Authority |
| TXT        | TXT Records        |
- A Canonical Name or CNAME record is **a type of DNS record that maps an alias name to a true or canonical domain name**.
- The DNS 'start of authority' (SOA) record **stores important information about a domain or zone such as the email address of the administrator, when the domain was last updated, and how long the server should wait between refreshes**. All DNS zones need an SOA record in order to conform to IETF standards.
- TXT records are **a type of Domain Name System (DNS) record in text format, which contain information about your domain**. TXT records contain information that helps external network servers and services handle outgoing email from your domain.

## dnsdumpster

Online tool.

 ![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/655c1ce6c3bb53e6252128b6da135ba9.png)

DNSDumpster will also represent the collected information graphically. DNSDumpster displayed the data from the table earlier as a graph. You can see the DNS and MX branching to their respective servers and also showing the IP addresses.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/844a43bc37547bbb13f94894605992d3.png)  

There is currently a beta feature that allows you to export the graph as well. You can manipulate the graph and move blocks around if needed.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/eb95e62815b6ace3c823fd0159884731.png)


## Shodan.io

Shodan.io tries to connect to every device reachable online to build a search engine of connected “things” in contrast with a search engine for web pages. Once it gets a response, it collects all the information related to the service and saves it in the database to make it searchable. Consider the saved record of one of tryhackme.com’s servers.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/a8ac6c22a64b8413ee8d02c2224eddac.png)  

This record shows a web server; however, as mentioned already, Shodan.io collects information related to any device it can find connected online. Searching for `tryhackme.com` on Shodan.io will display at least the record shown in the screenshot above. Via this Shodan.io search result, we can learn several things related to our search, such as:

- IP address
- hosting company
- geographic location
- server type and version