- never use with nothing, always precise what is recored
- -i INTERFACE: select interface to capture from
	- (ip a s)
- -w FILE: to save capture in FILE, usually with .pcap extension
- can use -r FILE to read from packets
- -c COUNT: to specify the number of packets (playing/captured)
- -n: stop DNS lookups
- -nn: stop DNS lookups and port resolving
- -v: verbose
- tcpdump host IP/HOSTNAME: to filter to exchanges with given IP/HOSTNAME
- port NUMBER: specify port number
- can filter protocol by mentionning it
	- ex: icmp, ip, ip6, udp, tcp, ...
- logical operators: and, or, not
- greater LENGTH/less LENGTH: to filter packets based on size
- man pcap-filter: to check filters available
- Filtering based on headers
	- proto: refers to protocl
	- expr: indicates byte offset
	- size: number of bytes that interests us: 1, 2, or 4
		- 1 by default
- tcp\[tcp-flags] to filters according to tcp flags
	- tcp-fin
	- tcp-syn
	- tcp-ack
	- tcp-rst
	- tcp-push
- Display commands:
	- -q: quiet mode
	- -e: prints link level layer
	- -A: show packet data in ascii
	- -xx: show packet data in hexa
	- -: show packet headers and data in hex + ascii

Tcpdump allows to capture network traffic into PCAP files.

## Capture basics

A basic capture can be made by simply typing `tcpdump` inside a terminal, but the result won't be of much use. On the contrary, we need to be more specific to filter the packets we want to capture.

The first thing is to sepcify the network interface, which can be done by using the `-i INTERFACE` flag.

The interfaces themselves can be visible by typing `ip a s`.

Then we specify in which file we want the information to be saved using the `-w FILE_NAME` flag. The extension will likely be `.pcap`.

After saving a file, it is possible to read it using the `-r FILE` flag.

When capturing, it is possible to restrict the number of packets captured by using the `-c COUNT` flag.

Then, if we don't want `tcpdump` to resolve the IP addresses into the DNS name assoicated or that common ports are translatedd to the usually associated protocol, we can use `n` to prevent the DNS lookups and `-nn` to prevent port translation.

The `-v`, `-vv` and `-vvv` flags can also be used to provide more verbose output.

## Filtering

Again, we need to filter more our captures to have interesting information that we can study.

Filtering by host is made by using the `host HOST_NAME/IP` clause. For example:

```bash
sudo tcpdump host example.com -w http.pcap
```

Filtering by port is made in a similar way with the  `port` keyword:

```shell-session
sudo tcpdump -i ens5 port 53 -n
```

Filtering by protocol is also possible by simply adding the protocol name in the command:

``` bash
sudo tcpdump -i ens5 icmp -n
```

These filters can be added together using logical operators, which are:
- `and`
- `or`
- `not`

## Advanced filtering

Here are some advanced ways of filtering, which restricts packets based on some more in-depth restrictions.

First, by packet length, which can be done by using the keywords:
- `greater LENGTH`
- `less LENGTH`

Filtering according to header bytes is done by following this synthax: `proto[expr:size`, where:
- proto refers to the protocol (arp, ehter, icmp, ip, ip6, tcp, ...)
- expr: indicates the byte offset
- size: indicates the number of bytes that interest us

Then we can apply binary conditions following that synthax, for example: `ether[0] & 1 != 0` or `ip[0] & 0xf != 5`.

The TCP flags can be accessed using the synthax `tcp[tcpflags]`, and can be compared with:
- `tcp-syn`
- `tcp-ack`
- `tcp-fin`
- `tcp-rst`
- `tcp-push`

which are simply the names of the flags used in the TCP protocol.

So a filtering made that way is perfectly ok: 

```bash
tcpdump "tcp[tcpflags] == tcp-syn"
```


## Specific packet display

It is possible to display packets in different way.

The `-q` flag allows for brief display. It shows:
- the source/destination IP addresses
- timestamp

To display link level header, we can use the `-e` flag. It allows to see DHCP and ARP packets.

Packets can be displayed as ASCII by using the `-a` flag.

The hexadecimal format can also be displayed by using the `-xx` flag.

Both can even be displayed by using the `-X` flag.

