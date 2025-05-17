- echo
	- $SHELL: show shell currently used
- whoami
- ls
	- -a
	- --help
	- 
- cd
- cat
- pwd
- find
	- -name
- grep
	- -C \<integer>
	- -i
- man
- untar file : `
```bash
tar -xvf filename
```
- wget:
```shell-session
wget http://MACHINE_IP:8000/myfile
```
- ps
	- aux
- top
- kill
	- -9
- systemd
- systemctl
	- start
	- stop
	- enable
	- disable
- CTRL +z
- fg
- add-apt-repository
- telnet: to connect to any server listening to a TCP port number
	- telnet MACHINE_IP PORT
- curl
	- -H: specify content typ
		- -H 'Content-Type: application/x-www-form-urlencoded'
	- -d specify parameters to the request
	- -X \<method>