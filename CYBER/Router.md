Connect networks and exchange data between them
![[Pasted image 20250228143539.png]] 

- [[Ports#Port Forwarding|Port Forwarding]]
- operate at Layer 3
- Firewalling
- Routing algorithms:
	- OSPF: Open Shortest Path First: calculate most efficient paths for data transmission
		- router exchange updates about states and connections => each router has complete map of network and can determine best routes
	- EIGRP: Enhanced Interior Gateway Protocol: Cisco protocol, to share info between routers, in order to find best routes
	- BGP: Border Gateway Protocol: primary routing protocol, 
	- RIP: routing protocol for small networks, share info on networks they can reach and number of hops required, creates table based on that