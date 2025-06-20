


 ![[Pasted image 20250227151439.png]]

- reliability
- error checking
- constant connection between 2 devices (connection-based), initialized via Three-way handshake
- prevents flooding of data
- Headers:
	- Source port
	- Destination Port
	- Source IP
	- Destination IP
	- Sequence Number: given number to the first piece of data when connection occurs
	- Acknowledgement Number: after sequence number to first piece, next ones will have sequence number + 1
	- Checksum: when data received, math done base on data, must match the number, otherwise data corrupt
	- Data: stores actual data
	- flag: determines how the packet should be handled by device
- Three-way handshake messages:
	- SYN: initial packet to initialize connection
	- SYN/ACK: sent by receiver to ack the synch attempt from the sender
	- ACK: used by client or server to ack that connection or series of packets have been correctly received
	- FIN: to cleanly close connection after it has been complete
	- RST: abruptly end all communication
- Reconstruct Data with Sequence/Ack numbers:

1. SYN - Client: Here's my Initial Sequence Number(ISN) to SYNchronise with (0)
2. SYN/ACK - Server: Here's my Initial Sequence Number (ISN) to SYNchronise with (5,000), and I ACKnowledge your initial number sequence (0)
3. ACK - Client: I ACKnowledge your Initial Sequence Number (ISN) of (5,000), here is some data that is my ISN+1 (0 + 1)

- Close connection: 
	- Client: FIN
	- Server: ACK + FIN
	- Client: ACK