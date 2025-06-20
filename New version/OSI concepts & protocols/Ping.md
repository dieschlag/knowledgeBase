
Command that sends an ICMP echo request (Type 8) to a remote system. If system is online and packet correctly routed, the remote system should send back an ICMP request Echo Reply (Type 0), received by the sender if properly routed/not blocked by firewall.

## Useful flags

-c \<number>: specify total count of packets // -n \<number> on a MS Windows system
-s \<number>: specify the number of data bytes sent, default is 56 making the total size of the packets 64 with the 8-byte header


