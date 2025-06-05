TELNET is the Teletype Network protocol, developed in 1969 to communicate with a remote system using a command-line interface (CLI). The `telnet` command uses this protocol for remote administration.

The default port is 23, and all data is sent in cleartext. The secure alternative is [[SSH]].

Since it uses TCP, `telnet` can be used to communicate with any running service using TCP without encryption. For exemple to connect to a service running to the port 80 of a given machine:

 `telnet MACHINE_IP PORT`