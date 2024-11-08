When we are targeting a network, we want to find out the following:
1. Which systems are up?
2. What services are running on these systems?

This note focuses on finding live computers.

This room explains the steps that Nmap carries out to discover the systems that are online before port-scanning. This stage is crucial because trying to port-scan offline systems will only waste time and create unnecessary noise on the network.

There are three ways nmap can discover live hosts (there's more):

1. ARP scan: This scan uses ARP requests to discover live hosts
2. ICMP scan: This scan uses ICMP requests to identify live hosts
3. TCP/UDP ping scan: This scan sends packets to TCP ports and UDP ports to determine live hosts.

We will also cover  `arp-scan` and `masscan` and explain how these scanners overlap with nmap's host discovery.

An nmap scan usually goes thru steps like these (depending on CLI args you provide):

1. Enumerate targets
2. Discover live hosts
3. Reverse DNS lookup
4. Scan ports
5. Detect versions
6. Detect OS
7. Traceroute
8. Scripts
9. Write output

## Subnetworks

Let's review a couple of terms. 
A _network segment_ is a group of computers connected using a shared medium. For instance, the medium can be the Ethernet switch or WiFi access point.
In an IP network, a _subnetwork_ is usually the equivalent of one or more network segments connected together and configured to use the same router. 

The network segment refers to a physical connection, while a subnetwork refers to a logical connection.

 A subnetwork, or simply a subnet, has its own IP address range and is connected to a more extensive network via a router. There might be a firewall enforcing security policies depending on each network.  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/aa787518e856e0094cb40da8399be0f3.png)

The figure above shows two types of subnets:

- Subnets with `/16`, which means that the subnet mask can be written as `255.255.0.0`. This subnet can have around 65 thousand hosts.
- Subnets with `/24`, which indicates that the subnet mask can be expressed as `255.255.255.0`. This subnet can have around 250 hosts.
(every octet can have a value of 0-255 so each octet more in a subnet just multiply)

Doing recon we want to discover more info about group of hosts or about a subnet.
If you are on the same subnet, your scanner can use ARP queries to discover live hosts.

An ARP query aims to get MAC addresses so that communication over the data link layer is possible.
However this information will also tell if the host is live or not.

If you are in a different subnet, in this case all packets from your scanner will be routed via the default gateway (router) to reach systems on another subnet.
However the ARP queries won't be routed and cannot cross the subnet router.

ARP is a data link protocol and ARP packets are bound to their subnet.

## Enumerating Targets

