- allows devices on separate network to securely communicate securely, creates dedicated path over Internet (**tunnel**), as if on same **private network**
- offers privacy (encryption to protect data) + anonimity
- technologies:
	- PPP: used by PPTP to allow authentification and encryption, use pricate/public key, need a match to connect, cannot leave the network by itself
	- PPTP: Point-to-Point Tunneling Protocol: allows data from PPP to travel, supported by most devices, weakly encrypted 
	- IPSec: Internet Protocol Security: encrypt data using IP framework, strong encryption, supported by many devices
	

VPN stands for Virtual Private Network.

With TCP/IP protocol suite, the main focus is that data is delivered. There is built-in mechanisms to deal with the loss of a router if a packet is not acknowledged.

But no mechanism is available to make sure that all data leaving or entering the computer is protected from disclosure and alteration.

A VPN allows to have this king of protection. It also allows to connect devices as if they were on the same network. The communication between a VPN server and a client is made using a tunnel and when trying to access the Internet, the data first goes through that tunnel. As a consequence, the other devices will not our IP address but the VPN server's IP address.