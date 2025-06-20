HTTP relies on TCP and uses port 80 by default. It sends traffic in cleartext that anyone can intercept and monitor.

HTTPS stands for Hypertext Transfer Protocol and is basically HTTP over TLS. So requesting a page will require the following steps:
- establish a TCP three-way handshake with the target server
- establish a TLS session
- communicate using HTTP protocol

After that, the HTTP packets will be encrypted and a user intercepting the communication will not be able to access the data.