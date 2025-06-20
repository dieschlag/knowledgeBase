- SSH: Secure Shell 
- use SSH client, most-likely based on OpenSSH, open source implementation of SSH
- supports public key and 2FA auth
- end-to-end encryption
- notifies of new server keys to protect agains man-in-the-middle attacks
- confidentiality and integrity
- secure "tunnel" for other protocol over ssh
- X11 Forwarding: allows to connect on Unix using GUi
	-  use ssh with -X
- add ssh key to remote: ssh-copy-id
- only owner can write and read (600), otherwise key cannot be used

One of the downside of using telnet to send commands to a computer over an unsecured network, is that information sent using it is sent using cleartext.

In 1995, Secure Shell protocol (SSH) was developed, as a first version named SSH-1 by Taty Ylönen. A more secure version in 1996, SSH-2 was released. In 1999, OpenBSD released OpenSSH, an open-source implementation of SSH, which is on what most libraries using SSH are based on.

The goal is o authenticate and encrypt connections over an unsecured network. It also allows for tunneling and port forwarding.

It runs on top of the TCP/IP protocol suite and incorporates authentification using public key cryptography, which relies on a pair of keys. One of the key is available to everyone, while the other is kept by the owner. Establishing the key owner's identity requires to have the private key that goes with the public key.

When a client tries to communicate using SSH, it locks the data using its own private key. The server receiving the data also locks the data with its own key and sends back the packet. The client then removes the first lock it put on the packet and sends it back again. The server then can remove the lock it put and access the data.

When establishing a SSH communication, the client requests a ssh connection specifying the public key to use. If the server has the public key, it will generate a random string, encrypt it using the public key and send it to the client. The client will decrypt it and make specific calculation to prove it has indeed decrypted the string sent by the server, before sending it back to the the server. If the correct string is sent back, the connection is established.

When connecting with the `ssh` commang, a prompt asks us to confirm the server's identity by checking the public key's signature of the server, since a man in the middle attack is possible.

These asymmetric keys also make it possible for the two sides of the connection to negotiate identical and shared symmetric keys for further encryption over the channel.

Key cryptography allows to authenticate connected devices such as SSH but a properly secured computer will still require authentication from the person using SSH (by providing a username and password for example).

Authentication can also be done using SSH keys. By default they are RSA keys but other types of keys can be used among:
- DSA (Digital Signature Algorithm): public-key cryptography algo specifically designed for digital signatures
- ECDSA (Elliptic Curve Digital Signature Algorithm): variant of DSA that uses elliptic curve cryptography
- ECDSA-SK (ECDSA with Security Key): extension of ECDSA, which incorporates hardware-based security keys for enhanced key protection
- Ed25519: public-key signature system using EdDSA (Edwards-curve Digital Signature Algorithm)
- Ed25519-SK (Ed25519 with Security Key): similar to ECDSA-SK in the sense that it uses security key private key protection

A private key must have specific permissions, which are 600, meaning that only the owner should be able to read or write to the private key.

Keys are stores inside of the .ssh folder located in the home folder of the user to whom the key belongs to. The authorized_keys file in this directory holds public kwys that are allowed to access the server if key authentication is enabled.

Then the person can execute commands on the remote machine as if they were doing so on their local machine.

## SFTP

SFTP stands for SSh File Transfer Protocol and allows secure file transfer. It is part of the SSH protocol and uses as well the port 22.