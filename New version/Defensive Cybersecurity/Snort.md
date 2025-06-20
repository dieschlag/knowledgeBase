- hybrid IDS
- Three modes:
	- Packet Sniffer: reads + displays network, helpful in monitoring
	- Packet Logging: real-time detection + alerting, can log traffic in PCAP
	- Network Intrusion Detection: generates alerts bases on rules defined
- built-in files stored in /etc/snort
- Rule format:
![[Pasted image 20250312020403.png]]
- add rules: add line in /etc/snort/rules/local.rules
- Use snort as live testing:
```shell-session
sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf
```
- Use on a PCAP:
```shell-session
sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf
```
