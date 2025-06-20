Subnetting: splitting network into smaller networks
![[Pasted image 20250228143708.png]]

- identified by a range of IP Addresses
- At home: generally only one subnet of 254 devices

- Network Address: Start of actual network: 192.168.1.0
- Host Address: IP address inside network: 192.168.1.100
- Default Gateway: special address able to send data to other networks: 192.168.1.0
- Broadcast Adress: 192.168.1.255
- To identify network, can write: `192.168.66.89/24`, /\<number> = number of the first bits wich are fixed in a host address for the network

For example with  `10.10.12.13/29`, we refer to the host 10.10.12.13 in a subnet of size 8 (32 - 29 = 3 bits so)

Linked to that example, if we consider the addresses of the network 10.10.12.8/29, we have the following addresses:
- 10.10.12.8: network address
- 10.10.12.9: first host address
- 10.10.12.14: last host address
- 10.10.12.15: broadcast address
## Private addresses:

- `10.0.0.0` - `10.255.255.255` (`10/8`)
- `172.16.0.0` - `172.31.255.255` (`172.16/12`)
- `192.168.0.0` - `192.168.255.255` (`192.168/16`)


