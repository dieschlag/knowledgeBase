Thanks to IP addresses, hosts on a network can be identified without amiguity. Now, we need protocols that enable processes on networked hosts to communicate.

To identify the process, we use port numbers. A port uses 2 octets, meaning it ranges between 1 and 65535. The port 0 is reserved

UDP and TCP are two protocols allowing that.

## UDP

User Datagram Protocol allows to reach a specific host on the target. It acts on the layer 4 and is connectionless, meaning that it does not provide a way to know if the packet has been delivered.

An piece of information sent over UDP is refered to as an UDP datagram. 

## TCP

Transmission Control Protocol is a connection-oriented transport protocol, meaning it ensures reliable data is transmitted by the different processes on the networked hosts. A connection is established before any data is sent between the two processes.

In TCP, each data octet has a sequence number, allowing the receiver to identify lost or duplicate packets and acknowledging the reception of data to the sender by sending him an acknowledgement number.

A TCP connection uses a three-way handshake. Two flags are used: SYN (Synchronize) and ACK (Acknowledgment).

The steps of the handshake are the following:

![[Pasted image 20250606161104.png]]

An unit of information sent via TCP is known as a TCP segment.

We can use `telnet` to connect to any listening server listening on TCP port number. More information can be found [[TELNET|here]].

