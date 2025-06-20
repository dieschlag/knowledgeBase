- open source tool written in Goland
- enumerates web directories, DNS subdomains, etc.
- between reconnaissance and scanning phases
- Enumerate directories
```shell
gobuster dir -u "http://www.example.thm" -w /path/to/wordlist
```
- can add -r to follow redirections when receiving a 301 code
- -x: to specify files enumerated (ex. .php, .js)
- use `gobuster dns` to enumerate subdomains
	- -c/--show-cname: show CNAME records
	- -i/--show-ips: show IPs
	- -r/--resolver: configures custom DNS server for resolving
	- -d/--domain: configures domain we want to enumerate
```shell
gobuster dns -d example.thm -w /path/to/wordlist
```
- to enumerate vhost, use `gobuster vhost`
	- -u/--url: specify url base for bruteforce vhost
	- --append-domain: appends the domain to each word in the list
	- -m/--method: specifies HTTP method for requests
	- --domain: appends a domain to each wordlist to form a valid hostname
	- --exclude-length: exults results based on length of response body
	- -r/--follow-redirect: follow HTTP redirects
```shell
gobuster vhost -u "http://example.thm" -w /path/to/wordlist
```
