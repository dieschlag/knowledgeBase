The Open Systems Intercommunication (OSI) model was developed by the International Organization for Standardisation (ISO) and describes how communication occurs in a computer network.

A smaller version of that model, called the [[TCP-IP Model|TCP/IP Model]].

The OSI Model is composed of seven layers:

![[Pasted image 20250606114903.png]]

## Application

This layer provides network services to end-users.

Ex of protocols: 
- [[HTTP]]
- [[FTP]]
- [[DNS & WHOIS|DNS]]
- [[POP3]] 
- [[SMTP]]
- [[IMAP]]

Secured version of these protocols exist since [[TLS]] was invented and led to these new protocols:
- [[HTTPS]]
- [[FTPS]]
- [[SFTP]]
- [[SMTPS, POP3S, IMAPS, FTPS]]
- [[POP3S]]
- [[IMAPS]]
## Presentation

This layer ensures data is delivered in a form the application can understand. It handles data encoding, compression and encryption.

Ex of protocols: 
- Unicode
- MIME
- JPEG
- PNG
- MPEG

## Session

This layer is responsible for maintaining and synchronizing communication between different apps running on different hosts. Establishing a connection is initiating a connection between apps and negotiating parameters for the session. Data synchronization ensures data is transmitted in the correct order and provides recovery in case of a failure.

Example of protocols: 
- NFS
- RPC

## Transport

This layer enables end-to-end communication between apps on different hosts.

Examples of protocol:
- UDP
- TCP

More information can be found on these two protocols [[UPD and TCP|here]].
## Network

This layer focuses on sending data between different networks. It handles logical addressing and routing.

Ex of protocols:
- IP
- ICMP
- IPSec

## Data Link

This layer focuses on transmitting between nodes on the same network. It provides an agreement on how to communicate between the different systems inside a network.

Data units exchanged at this layer are called frames.

Examples of protocol:
- Ethernet (302.3)
- WiFi (802.11)

Ethernet and WiFi addresses are MAC, for which more information can be found [[MAC Address|here]].

## Physical

This layer allows the physical connection between devices, including the medium used, and the definition of 0 and 1 digits sent on the network.

## Encapsulation

When going down each layer, an header is added to the unit of data received at a layer level. This process is known as encapsulation.

The different header added are displayed below with their names:

![[Pasted image 20250606161445.png]]

Important to notice is that for the data layer, an additional trailer is added at the end of the data unit received from layer 3.

The IP header is added at layer 3 and TCP/UDP header is added at layer 4.

