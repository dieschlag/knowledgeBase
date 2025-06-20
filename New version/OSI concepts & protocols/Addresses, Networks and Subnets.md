Each host on a network needs a unique identifier, otherwise there might me ambiguity when we want to reach a specific host. It like for a town where each house needs a specific address.

These addresses on a network are IP addresses, which can either be IPv4 or IPv6 addresses. 

These addresses are used as part of the layer 3 networking, with the use of public IP addresses. For layer 4, ports are used to identify a process. These ports are then used by the TCP and UDP protocols. More information on these protocols can be found [[UPD and TCP|here]].

## IPv4


IPv4 addresses are made of four octets, so 32 bits. Each octet can be represented as a number, so we can display an IPv4 address as such:

**192.169.1.1**

Each number range from 0 to 255. There are 2^32 IPv4 addresses in the world. For a given network, the IPv4 smallest address is considered as the gateway address and the address with the highest numbers is the broadcast address of the network.

For example, if we consider the host: 192.168.66.89/24, the /24 means that the IP addresses of the other hosts on the same network have their 24 first bits fixed. As a consequence, in this situation, the address 192.168.66.255 is the broadcast address and 192.168.66.0 is the network address.

This /24 can be written as a subnet mask which is written in the same way as an IP address but with the network bits set as 1. In this situation the subnet mask associated to /24 is:

255.255.255.0

On windows, we can find our IP address when using cmd.exe by typing `ipconfig`. On Linux, we use `ifconfig`. We can use as well `ip a s` on Linux to see what the network card of the host is.

IPv4 addresses can either be public or private. RFC 1918 defines three ranges of private IP addresses:
- 10.0.0.0 - 10.255.255.255 (10/8)
- 172.16.0.0 - 172.31.255.255 (172.16/12)
- 192.168.0.0 - 192.168.255.255 (192.168/16)

The principal of a private IP address is that it cannot be reached from outside world. For a private IP address to access the Internet, the router must have a public IP address and support NAT (Network Address Translation)

## DHCP

When we want to access a network, we need to define the following elements:
- IP address along with the subnet mask
- router (aka the gateway of the network)
- DNS server

It is possible to configure them manually, which can be a good idea for servers, since they are not supposed to change of network, so these informations can be fixed for a server and can be helpful to indicate easily to other servers/services where to send information.

But for some network, like a coffee shop WiFI, we need to define these elements dynamically with new devices connecting to the network and leaving it.

This dynamic addressing of machines on a network is made using Dynamic Host Configuration Protocol (DHCP). 

This process is an application-level protocol that relies on UDP, with the server listening on UDP port 67 and the client sending to UDP port 68. Most phones and laptops are configured to use DHCP.

DHCP follows four steps:
- DHCP Discover: The client broadcasts a DHCPDISCOVER message seeking the local DHCP server
- DHCP Offer: The servers responds with a DHCPOFFER message and an IP address available for the client
- DHCP Request: The client responds with a DHCPREQUEST message to indicate it has accepted the proposed IP address
- DHCP Acknowledge: The server responds with a DHCPACK message to confirm that the offered IP address is assigned to the client.

![[Pasted image 20250606164322.png]]

Since the client has no IP address (it only has a MAC address) when sending the first and third packets, it sends its packet fom the IP address 0.0.0.0 to the broadcast IP address 255.255.255.255.

For the link layer, the client sends to the broadcast MAC address `ff:ff:ff:ff:ff:ff`. The server uses the client MAC address to communicate with it.

## ARP

An IP packet is encapsulated within a data link fram at layer 2, where the most common protocols are Ethernet (IEE 802.3) and WiFi (IEEE 802.11).

In order to create the data link header, it needs to know the MAC address of the target host. More information can be found on MAC addresses [[MAC Address|here]].

Address Resolution Protocol (ARP) makes it possible to find the MAC address of another device on the Ethernet. Three steps occur:
- An ARP Request is sent from the MAC address of the requester to the broadcast MAC address ( ff : ff : ff : ff : ff : ff ), asking the host with a given IP address to respond.
- The receiver answers with an ARP Reply to the requesting MAC address with its MAC address included in the response. At this point, the two can communicate using data link layer frames
- The answer is cached by the sender as ARP Cache.

## ICMP

Internet Control Message Protocol is used for diagnostics and error reporting. Two popular commands are:
- `ping`
- `traceroute`

More information on these two commands can be found [[Ping|here]] for `ping` and [[Traceroute|here]] for `traceroute`.

## Routing

It is required to have specific algorithms to route packets between networks, that are going to be used by the routers to figure out which link to use to send a frame.

Some algorithms are:

- Open Shortest Path First (OSPF): It allows to share information about the network toplogy, by having routers exchange updates on the state of their connected links and networks, so each router has a complete map of the network and can determine the shortest route.

- Enhanced Interior Gateway Routing Protocol (EIGRP): It is a proprietary Cisco routing protocol. It allows routers to share information on the networks they can reach and the delay to reach them, who will use this information to determine the best path

- Border Gateway Protocol (BGP): It is the primary routing protocol on the Internet and allows different networks to exchange routing information and establish paths to travel between networks, providing efficient routing, even across multiple networks. 

- Routing Information Protocol (RIP): It is a simple protocol, used in small networks. Routers share info about the networks and the number of hops to reach them to build a routing table and choose the route with the fewest of hops to reach the destination.

## NAT

With the increase in the number of devices used, the IPv4 system was doomed to be lacking free IP addresses. The solution to that issue is using Network Address Translation.

The idea behind it is that we use one public address to provide Internet to many private IP addresses. This protocol is supported at the level or routers

