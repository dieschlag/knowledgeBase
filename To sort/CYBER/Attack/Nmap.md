- communication made via ports
	- 65535 ports
	- NETBIOS: 139
	- SMB: 445
	- to scan: nmap
- Switches:
	- -sn: discover live hosts
	- -sS: Syn Scan
	- -sU: UDP Scan
	- -sT: TCP connect scan
	- -O: detect OS
	- -sV: get service versions
	- -sL: List Scan: simply lists hosts of the network, without sending packets to the target hosts. It uses reverse-DNS resolution on the hosts to know their name.
	- -v/-vv: verbose
	- v\<number>: other way to add verbose
	- -d\<integer>: debug mode with level
	- -oA: save in three major formats
	- -oG: save in grepable format
	- -oN: normal output
	- -oX: in xml
	- -A: agressive mode
	- -F: fast mode, scans 100 most common ports
	- -T\<integer> (T1, T2, etc.): "timing" templates, increase scan speed
		- number up => takes longer, because more stealth
	- -p \<integer>: scan port
	- -p \<integer>-\<integer>: scan range 
	- -p-: scan all ports
	- --script: launch scan script
	- --script=\<category>: launch script of given cateory
	- --script=\<script-name>
	- --script-args script-name.arg1,script-name.arg2 
	- --min-parallelism/max-parallelism: number of parallel probes used
- TCP Connect Scans: -sT
	- TCP Handshake: SYN => SYN + ACK => ACK
	- if port closed: taget returns RST (reset flag)
		- RFC 9293
	- when firewall simply configured to drop packets, sender receives nothing
	- configure firewall to nonetheless sends RST back
```
	iptables -I INPUT -p tcp --dport <port> -j REJECT --reject-with tcp-reset
```
- SYN Scan: -sS
	- "Half-open"/stealth scans: SYN => SYN/ACK => RST
		- some apps only log once the connection has been made
		- idem for old intrusion detection systems
		- faster
		- require sudo
		- can take down unstable services, issue when pentest on production
		- default when running nmap with sudo permissions
- UDP Scans: -sU
	-  reminder: no handshake
	- more difficult/slower to scan (~20 min for 1000 most common ports)
	- if port open, should receive nothing => `open|filtered`
		- but could be firewalled
		- if no response, request sent a second time, marks it `open|filtered` and moves on
		- if receives response: marked as `open`
		- if port closed: receives a ICMP ping that port is unreachable
	- generally used with `--top-ports <integer>` to scan the \<integer> most common ports
		- also allows to send UDP packets with content more likely to receive a response
		- instead: empty request, raw UDP packet

- Scans with firewall evasion: many firewalls drop TCP packets with the SYN flag set/ICMP ping
	- but modern systems are savvy
	- Types of scans available:
		- NULL scans: -sN
			- TCP request with no flags set
			- target responds with RST if closed
			- expects no response if open
		- FIN Scans: -sF: 
			- TCP request with only `FIN` flag set
			- expects RST if port closed
			- expects no response if open
		- Xmas scans: -sX
			- sends malformed TCP request
			- expects RST closed
			- name: appearance of blinking xmas tree when packet viewed in wireshark
			- expects no response if open
		- -Pn: no ping before scanning, prevent firewall block
			- takes longer (will verify port of unreachable hosts)
		- -f: fragment packets, less likely to be detected
		- --mtu \<number>: defines maximum transmission unit for packets
			- must multiple of 8
		- --scan-delay \<time>ms: adds delay between packets sent
			- avoid time-based triggers
		- --badsum: generate invalid checksum packets
			- TCP/IP stack would drop it
			- firewall may respond automatically => detects presence of firewall
			- --data-length \<integer>: adds arbitrary amount of data at the end of packets  
	- RFC 793: for malformed packets:
		- RST if closed
			- no response if open
			- not always respected:
				- Windows/Cisco devices: always respond with RST if packet malformed
- ICMP Network Scanning: -sn (ping sweep)
	- in black box assignment, need **map** of network
	- accepts `-` range or CIDR (`\`) notation to specify network to scan
- Nmap Scripting Engine (NSE):
	- scripts written in Lua (language)
	- categories:
		- safe: not affect target
		- intrusive: likely to affect target
		- vuln: scan for vuln
		- exploit: attempt to exploit vuln
		- auth: attempt to bypasse auth
		- brute: bruteforce credentials
		- discovery: query running services to get more info
	- list of scripts [here][https://nmap.org/]
	- search for scripts:
	- website
		- stored in `/usr/share/nmap/scripts`
		- `/usr/share/nmap/scripts/script.db`: db/formatted .txt of filenames + categories
			- grep in it
			- to search for categories `grep "category" file path`
		- can use ls with templates: `ls path/*name*`
	- if download a script (wget etc.), then `nmap --script-updatedb` to update script.db

`nmap` is a very efficient-tool when it comes to network scanning.

It allows to discover live hosts.  Nmap uses multiple ways to specify its targets:
- an IP range can be specified using `-`, for example: `192.168.0.1-10`
- IP subnet range using `/`, for example: `192.168.0.1/24`
- hostname by simply specifying it after the command (like for `ping`).

To discover the online hosts we can use the `sn flag`.

Depending on the flag, it can be necessary to execute nmap with `root` privileges because some scanning require to write custom packets.

When scanning the local network, `nmap` check for live hosts simply by simply sending ARP requests.

When scanning a remote network, `nmap` will try to ping the device. If it is not responding after two ping, it will send a SYN TCP request and if still not responding after two requests, it will send a ACK TCP request.

We can have more control over what TCP packets are sent using the `-PS[portlist]`, `-PA[portlist]` and `-PU[portlist]` flags, which allow us to define port ranges for which TCP SYN, TCP ACK and UDP packets will be sent.  

To have a better idea of what `nmap` is going to scan when we give a specific host or range to analyze, the scan list can be returned given by using `-sL` flag. No scan will be performed, only the list will be returned.

Before scanning, `nmap` makes a first host discovery using ICMP requests or ARP requests. It is possible to force the scan as if all hosts are considered up by adding the `-Pn` flag.

## Port scanning


As a reminder, there are 65 535 ports for UDP and TCP.

To know if a TCP port is open, one way is to try to make the three-way TCP handshake with that port.

That's what what `nmap -sT` does when applied to a host. If the port is open and the 3-way handshake succeeds, nmap simply closes the connection.

Instead of trying to make the full connection, we can also just try the first step of the 3-way handshake, and just sending a SYN request. This can be done using the `-sS` flag.

The same thing can be done for UDP ports by using the `-sU` flag. If the post is closed, we expect an ICMP destination unreachable response.

Generally, Nmap sans the most common 1000 ports but other mods are possible:
- `-F` flag can be used to only scan the 100 most common ports
- `-p[range]` allows to specify a range of port scans. For example `-p10-1024` allows to scan port 10 to port 1024, `-p-25` will scan ports between 1 and 25, `-p-` will scan all ports

## Version scanning

`nmap` allows for version scanning of different elements. 

### Os Detection

The OS detection can be done with the `-O` flag. Nmap will try to make an educated guess about the target OS based on different indicators.

### Service and Version Detection

When we discover several ports open and want to know more on the services and versions that are run. This can be done by using the `-sV` flag. 

### Both

To do both scans at once, we can use the `-A` flag.


## Timing

The speed of a scan could trigger an IDS or other security solutions. Nmap gives 6 timing templates: 
- paranoid (0)
- sneaky (1)
- polite (2)
- normal (3)
- aggressive (4)
- insane (5)

To chose one of this template, we can use the `-T<number>` flag.

For specifics, if we scan the 100 most common, the amount of time these scans take is:

- T0: 9.8 hours
- T1: 27.53 min
- T2: 40.56 sec
- T3: 0.15 sec
- T4: 0.13 sec

Another option is the number of parallel probes we use, which can be controlled with the `--min-parallelism` and `--max-parallelism` flags. By default, `nmap` defines the number of probes based on the network performance.

A similar can be achieved using `--min-rate <number>` and `--max-rate <number>` flags. They control the max and min rates at which nmap send packets. It is counted in numbers of packets per second.

Another option is to set the `--host-timeout <time>` flag.

## Verbose and Debug

Verbose is directly controlled by the `-v` flag, which can be extended from `-v` to `-v4`.

It works the same way for debugging, with the `-d` flag and ranges from `-d` to `-d9`.

## Output format

Several formats are possible to store the results of a scan:
- `-oN <filename>`: normal output
- `-oX <filename>`: XML output
- `-oG <filename>`: grepable output, which is useful when combined with `grep` and `awk`. More information on AWK [[AWK|here]].
- `-oA <basename>`: outputs in all major formats

