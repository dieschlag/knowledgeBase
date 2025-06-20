![[Pasted image 20250227145825.png]]
- Trailer: to check that data has not been corrupted on transport + security, cannot intercept data without breaking the trailer
- Headers example:
	- Time To Live: to not clog up the network if ti never reaches a host
	- Checksum: integrity checking, check if corrupt
	- Source Address: Ip Address of source
	- Destination Address: target IP
- 