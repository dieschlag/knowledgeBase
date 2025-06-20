- DNS: Domain Name System
- request to website
- computer checks local "Hosts File" to see if domain-ip match
- computer checks local DNS cache
- send request to recursive DNS (known to the router/computer)
- server checks cache
- not in cache => request to root name server
	- before 13 root name DNS servers
	- now more but still accessible via same IPs
	- root name servers keep track of TLD (Top Level Domains)
	- TLD: redirects request to appropriate authoritive server
		- TLD keeps track of Authoritative name servers
		- Split into extensions: 
			- ex: domain.com => redirect to TLD handling .com domains
		- Two types:
			- gLTD: Generic Top Level Domain
				- .com (commercial), .org (organisation)
			- ccTLD: Country Code Top Level Domain
				- .ca: Canada, etc.
	- Second-Level-Domain
			- max 63 caracters
			- can only use a-z + 0-9 + hypens (except begin/end)
	- Subdomain:
		- left side of SLD with .
		- same restrictions as SLD
		- can use multiple subdomains
		- max total length: 253
		- no limit on number of subdomains (only length)

- Record Types:
	- A: IPv4
	- AAAA: IPv6
	- CNAME: resolve to another domain name (will require another DNS request)
	- MX: address of servers handling email, priority flag to tell in which order to try the servers, if needs to send to backup if main is down
	- TXT: text-based data
		- can store list of servers wth authority to send email on behalf domain
		- ...
- dig: allows to query recursive DNS
```
dig <domain> @<dns-server-ip>
```
- TTL: Time to live
	- displayed in seconds in dig



DNS is responsible for mapping a domain name to a private IP address

It operates at the layer 7 of the OSI model and uses UDP port 53 by default and TCP as a fallback.

Several records exist for a DNS, including:
- A record: maps a hostname to one or several IP addresses
- AAAA record: same thing  but for IPv6
- CNAME record: maps a domain to another domain
- MX Record: specifies the mail server for 

With these records, we need a way to assert authority to set what these different records are actually mapping. Someone registering a domain name receives that power

We can register for a given number of years for a fee and we are required to provide accurate information, which will be displayed as part of a WHOIS record. 

A WHOIS record can be accesses using the command `whois`, which will display information about the entity that registered the domain name (like email, phone number, address, etc.).

