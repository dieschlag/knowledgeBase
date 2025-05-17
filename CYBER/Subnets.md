Subnetting: splitting network into smaller networks
![[Pasted image 20250228143708.png]]

- identified by a range of IP Addresses
- At home: generally only one subnet of 254 devices

- Network Address: Start of actual network: 192.168.1.0
- Host Address: IP address inside network: 192.168.1.100
- Default Gateway: special address able to send data to other networks: 192.168.1.0
- Broadcast Adress: 192.168.1.255
- To identify network, can write: `192.168.66.89/24`, /\<number> = number of bits wich are fixed in a host address for the network

## Private addresses:

- `10.0.0.0` - `10.255.255.255` (`10/8`)
- `172.16.0.0` - `172.31.255.255` (`172.16/12`)
- `192.168.0.0` - `192.168.255.255` (`192.168/16`)


