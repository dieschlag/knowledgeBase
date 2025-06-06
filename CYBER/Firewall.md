- Permit or deny traffic entry based on rules
	- destination
	- source
	- protocol
	- ...
- Stateful: uses **entire information from connection**, not looking at packets individually, takes more ressources
	- layer 3/4
- Stateless: uses a set of rules for **individual packets**, uses fewer ressources, dumber, great when receiving large amount of traffic from a set of hosts
	- layer 3/4
- Proxy Firewalls: Application-level gateway
- Next-Generation Firewalls
	- operates layer 3 to 7
	- decrypts TLS/SSL
	- heuristic analysis
- Rules in Firewall
	- Components
		- Source address
		- Destination address
		- port
		- protocol
		- Action
		- Direction: apply rule to incoming or outgoing traffic
	- Actions:
		- Allow
		- Deny
		- Forward: redirects to other subnet/host
			- redirect all incoming HTTP to port 80 of web server