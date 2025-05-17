List of listeners like netcat that can be used to handle shell connection
- Rlwrap: uses GNU readline, provide history
- ncat: improved version of netcat, distributed by the NMAP project, provides encryption
```shell-session
ncat --ssl -lvnp 4444
```
- Socat: allows to create a socket between two data sources
```shell-session
socat -d -d TCP-LISTEN:443 STDOUT
```
