Netcat can be used as a client that connects to a listening port or as a server that listens to a given port. It supports both UDP and TCP protocols.

It can be used to connect to a listening port on a remote machine to collect its banner. 

Example: to connect to a remote machine on a given port:

`nc MACHINE_IP PORT`

Then we can make HTTP request to know the host:

``` shell
pentester@TryHackMe$ nc 10.10.249.45 80 
GET / HTTP/1.1 
host: netcat  

HTTP/1.1 200 OK 
Server: nginx/1.6.2 
Date: Tue, 17 Aug 2021 11:39:49 GMT 
Content-Type: text/html 
Content-Length: 867 
Last-Modified: Tue, 17 Aug 2021 11:12:16 GMT 
Connection: keep-alive 
ETag: "611b9990-363" 
Accept-Ranges: bytes ...
```

## Useful tags

-l: listening mode
-p: specify the port number
-n: numeric only, no resolution of hostnames via DNS
-v: verbose output
-vv: very verbose
-k: keep listening after disconnects

Note: port numbers less than 1024 require root privileges to listen on

Classic command to listen on server side is: `nc -vnlp PORT_NUMBER`

