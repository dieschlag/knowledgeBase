- TLS: Transport Layer Security
- need for secure communication, otherwise simply need a capture-packet tool to retrieve people's credentials
- upgrade to SSL (Secure Socket Layer) 3.0
- 2018: TLS 1.3
- operates at transport layer
- server admin creates Certificate Signing Request (CSR), submits it to Certificate Authority (CA), CA verifies CSR and issues digital certificate.
	- is added to [[CT logs]]
- once received, can be used to identify the server to others, who can conform the validity of the signature
- Let's Encrypt: get certificate signed for free

TLS stands for Transport Layer Securities that are designed to provide security at the transport layer. It was derived from a protocol named Secure Socket Layer (SSL).

TLS ensures that no third party may eavesdrop or tamper a message.

The goal of TLS is to achieve:
 - security
 - privacy
 - integrity
 - authenticity with the use of cryptography and certificates

TLS runs in the presentation layer ans is composed of two layer: the TLS record and the TLS handshake protocols.

TLS does not run in any of the layers of the OSI model or the TCP/IP model. It runs on top of some reliable transport protocol, implying it can run over the transport layer. But it also allows encryption to higher layers, which is normally the function of the presentation layer. But generally, it is used by apps as if it is running at the transport layer ans is is composed of two layers: the TLS record and the TLS handshake protocols.

A close related protocol is the Datagram Transport Layer Security that provides security to datagram-based applications.

TLS is a proposed Intenet Engineering Task Force (IETF) standard, of current version 1.3. 

Applications can communicate with or without TLS, so the client first need to request a TLS connection. One way to do so is to designate a specific port to use to communicate with a given protocol over TLS. That's why port 80 is typically used for HTTP and port 443 for HTTPS.

TLS has been implemented in many protocol, which benefited of a major security upgrade.

The first step for every server that needs to identify itself is to get a signed TLS Certificate. The server admin creates a Certificate Signing Request (CSR) and submits it to a Certificate Authority (CA). The CA verifies the CSR and issues a digital certificate. 

For a host to confirm the validity of a signed certificates, the certificates of the signing authority need to be installed on the host. This is equivalent to recognizing the stamps of administrative authorities in the real world.
Once the client and server have agreed to use TLS, they negotiate a stateful connection using a TLS handshake.

## TLS handshake

### Negotiation phase
1) the client send a ClientHello message specifying:
	- the highest TLS protocol version it supports,
	- a random number
	- a list of suggested cipher suites and suggested compression methods
	- if the client attempts to perform a resumed handshake, it may send as well a session ID
2) the server answers with a ServerHello, containing:
	- the chosen protocol version, should be the highest one that client proposed
	- a random number
	- cipher suite and compression method from what the client proposed
	- a session ID to confirm or resume handshakes
3) Server send a Certificate message (depending on the cipher suite, could be omited)
4) Server sends a ServerKeyExchange message ( could be omited as well)
5) Server sends a ServerHelloDone indicating the handshake negotiation is done
6) Client answers with a ClientKeyExchange message, which may contain a PreMasterSecret, a public key or nothing (depends on the cipher). The PreMasterKey is used to encrypt the public key of the server certificate
7) Client and server use the random numbers and PreMasterSecret to compute a common secret, called the "master secret". All other key data is derived from that master secret, which is passed through a pseudorandom function.

### ChangeCipherSpec

After the negotiation, the client sends a ChangeCipherSec record, telling the server that everything from now on will be authenticated (and encrypted if such params were present in the server certificate). 

The ChangeCipherSec is a record-level protocol (i.e responsible for securing application data and verify its integrity) with content type 20.

The client sends an authenticated and encrypted Finished message, containing a hash and a MAC over the previous handshake messages

The server will attempt to decrypt the client's Finished message and verify the hash/MAX, If it fails to do so, the handshake is considered to have failed and the connection should be terminated.

Then the server sends as well a ChangeCipherSpec, for the same purpose as the client

### Application phase

At this point, after negotiating and validation thanks to the ChangeCipherSpec, the handshake is complete and the application protocol is enables, with content type 23.

Messages between client and server will be autenticated and encrypted like in their Finished message.

Otherwise the content type will return 25 and the client will not authenticate.