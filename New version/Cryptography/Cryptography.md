- Cryptography: science of scuring communication and data using codes and ciphers
- Cryptanalysis: study of methods to break cryptographic security systems
- Plaintext: original readable message
- Ciphertext: scrambled, unredable message after encryption
- Cipher: algorithm used to convert plaintext to cyphertext
- Key: string of bits used to encrypt/decrypt data
- Encryption: process of converting plaintext into ciphertext
- Decryption: process of converting ciphertext into plaintext
- Caesar Cypher
- [[Symmetric Encryption]]
- [[Assymetric Encryption]]
- Brute-Force Attack: attack method trying every possible key to decrypt
- Dictionnary Attack: attack method, attacker tries dictionnary words or combinations of them
	- For hashing: [[John The Ripper]]
- Digital Signature (encryption with private key, decryption with public key)
![[Pasted image 20250311001205.png]]
- Certificates
	- root CA, trusted on system install, etc.
- [[Hashing]]

Cryptography is defined as the practice and study of techniques for secure communication and data protection in the presence of adversaries and third parties.

It aims to protect confidentiality, integrity and authenticity.

It is used daily in most systems and regulation applies. For example, paying systems need to respect the PCI DSS (Payment Card Industry Data Security Standard). And in other countries like in european countries, companies need to comply with the GDPR.

## Common vocabulary

Plaintext refers to the readable data we want to encrypt. It can be anything.

This plaintext is then passed through an encryption function along with a proper key. The returned element is called a ciphertext. 

The key refers to a string of bits the cipher uses to encrypt and decrypt data. The key can be public, or assymetric.

The encryption function is part of the cipher. The cipher refers to an algoithm that allows to turn plaintext into ciphertext and vice versa.

To recover the plaintext, we must pass the ciphertext alog with the proper key to a decryption function.

## Types of encryption

Encryption can either be symetric or assymetric. We can also talk about symmetric or asymetric cryptography. 

### Symetric encryption

With this type, the same key is used to encrypt and decrypt data. This key is called a private key and must be kept secret. When communicating between parties, it is required to communicate the key through a secure channel.

![[Pasted image 20250618212053.png]]

Some examples of symmetric encryption are:
- [[(TODO) DES]]
- [[(TODO) 3DES]]
- [[(TODO) AES]]

Asymmetric encryption:

With this type, a pair of keys is used, one to encrypt and the other to decrypt. The data is encrypted using the public key, that's why it is generally refered to as public key cryptography.

![[Pasted image 20250618212044.png]]

This type tends to be slower and use larger keys than symmetric encryption. That's why asymmetric encryption is used to negotiate and agree on symmetric encryption ciphers and keys. The question is how can we agree on a key between a client and a server without sending the key in plaintext.

What happens is that the client asks the server for its public key, and encrypts the data using that public key. Only the server has the private key and is able to access the message you sent. In reality, certificates and digital signatures are also required. 

[[RSA]] uses 2048-bit, 3072 and 4096 bit keys with 2048-bit as the recommended minimum key size.

[[Diffie-Hellman]] also has a recommended minimum key-size of 2048 bit, but also uses 3072-bit and 4096-bit keys for enhanced security.

ECC can achieve security with shorte keys. With a 256-bit key, ECC provides the same level of security then a 3072-bit RSA key.

## Digital signatures

Digital signatures provide a way to verify the authenticity of a digital message or document. Using asymmetric cryptography, this is done by producing a signature with the private key, which can be verified using the public key.

When talking about documents, a hash of the latter can simply be encrypted using your private key, which will then be decrypted using the public key by the client to prove the document's integrity.

## Certificates

Certificates are linked to digital signatures and are used when dealing with HTTPS to answer the question: how can we know the server we are talking to is indeed the right server?

The web server has a certificate that confirms it is the correct server. These certificates have a chain of trust, starting with a root CA (Certificate Authority). From install time of our device, web browser or OS, various root CAs are truster. Certificates are trusted only when these root CAs say they trust the organisation that signed them.

So the chain is:

certificate signed by the CA => CA trusts the organisation => the browser trusts the CA

Getting easily a certificate can be done easily thanks to Let's Encrypt for free 

## PGP and GPG


PGP stands for Pretty Good Privacy and is software that implements encryption in order to encrypt file, perform digital signing and more. 

GPG or GnuPG is an open-source implementation of the OpenPGP standard.

GPG is commonly used in email to protect the confidentiality of email messages, to sign them and confirm their integrity.

It relies on a key that can be generated using the `gpg --full-gen-key` command. These keys are protected with a password in a similar way that SHS keys are protected. 