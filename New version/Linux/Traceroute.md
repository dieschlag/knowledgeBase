Traceroute traces the route taken by packets from our system to another host, it determines the IP addresses of the routers that a packet traverses, allowing to determine the number of hops required. The route might change since it is determined dynamically. It is mostly true when send packets outside of local network.

There is no direct way to discover the path but it is possible by using small TTL. When a router receives a packet, it decrements the TTL by one before passing it to the next router. If it reaches 0, an ICMP Time-to-Live (ICMP Type 11) exceeded is sent back to the original sender.

To note: not all routers are configured to send back this specific packet to the original sender when TTL reaches 0.

On Linux: 
- first send UDP packets within IP packets of TTL 1,
- then send a packet of TTL 2
- ...

