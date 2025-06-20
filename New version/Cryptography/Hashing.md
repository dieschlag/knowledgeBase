- Different from encryption
- no key and meant to be impossible, or computationally impossible, to go from output to input  
- [[MD5]]
- [[SHA-1]]
- [[SHA-2]]
- not secure if two files have same hash
- generally outuput is encoded after hashing, generally in hexa format
- Store passwords as hash, not as plaintext
- Hash Collision: two diff inputs give same output
	- consequence of the pigeonhole effect, if you have more elements than containers, some containers must contain more than one element
- Rainbow tables: Tables matching plaintext to hash output, used for password cracking
	- To prevent rainbow table attacks: use salt before hashing, random value added to beginning or end of password before hashing, one salt per user
- Hashing can be used for integrity checking, to check if hash of file downloaded matches expected file hash
- [[HMAC]]s
- Dictionnary Attack can be done with [[John The Ripper]]


Hashing is different from encryption. There is no key and it's meant to be impossible (or computationnaly imparactical) to go from the outpout back to the input.

The output of a hash function is called a digest and has a fixed size. A good hashing algorithm would be relatively fast and very slow to reverse, and any change in the data, even a single bit, would cause a significant change in the output.

Hashing help to protect data's integrity and ensure password confidentiality. For example when logging to a platform, the server does not record our password but a hash of our password. The server will calculate the hash value of the password we transmitted and compare it to the one stored in the database.

## Hash collision

A hash collision is when two different inputs give the same output. 

Hash functions are designed to prevent an attacker from being able to create a collision intentionally.

But collisions are inevitable because of the pigeonhole effect, the hashing function simply ensure that the probability of a collision is negligeable.

MD5 and SHA1 are now considered insecure due to the ability to engineer hash collisions easily.

## Password storage

Hashing is used for password storage. The goal is that even if the an attacker gets access to the database, he will not be able to use the information.

Two principles must then be followed:
- do not store passwords in plaintext
- use strong hashing algorithms

One issue when just storing the hashed password is that two users with the same password will have the same hash. It is then possible to create a Rainbow Table to break the hashes.

A Rainbow Table is a lookup table of hashes to plaintexts. It trades the time it takes to crack for hard disk space and creation time of the table.

To protect against that, salts are used which is a randomly value stored in the database and should be unique to each user. The salt is added either to the beggining or to the end of the password, meaning that every user will have a different password hash even if they have the same password. Salts don't need to be kept private.

## Passwords in OS

### Linux Passwords

Passwords are stored in /etc/shadow, which is normally readable by root.

They used to be stored in /etc/passwd which was readable by everyone.

Each line of the shadow file contains 9 fields, seperated by `:`.

The different fields are in order:
- the username
- password with the format `$id$salt$hash`
- last password change expressed in the number of days since Jan 1, 1970
- minimum number of days between password changes, can be 0
- maximum number of days the password is valid, after that the user if forced to change his password
- warn number, defining the number of days before the password expires that the user is warned
- the number of days after password expires that the account is disabled
- the date of expiration of the account expressed as the number of days since Jan 1, 1970

The id indicates the hashing algo and can be either:
- `$1$`: MD5
- `$2$`: Blowfish
- `$2y$`: Blowfish
- `$5$`: SHA-256
- `$6$`: SHA-512
- `$y$`: yescrypt

### MS Windows Passwords

MS Windows Passwords are stored in the SAM (Security Accounts Manager) and are hashed using NTLM, a variant of MD4, producing hashes looking similar to MD4 and MD5. We need to rely on context to know which type it is.

The hashes are split into NT hashes and LM hashes.


## Cracking passwords

To crack passwords, we simply generate many hashes and hope for a match. If a salt is used, we need to generate even more hashes to try different salts.

This operation can be done using tools like Hashcat or John the Ripper. It is better to crack passwords with GPU instead of CPU, that's why cracking on a VM (which mostly has CPU) is not the best idea. By default, John uses CPU, but better performance can be achieved using GPU.

## Integrity Checking

Hashing can be used to check that files have not changed, since the purpose of hashing is that any small change lead to very different hash. 

This is used when downloading files.


### HMACs

HMAC stands for Keyed-Hash Message Authentication Code. It is a type of message authentication code that uses a cryptographic hash function with a secret key to verify the authenticity and integrity of data. 

The steps are the following:

![[Pasted image 20250619102905.png]]

What happens is that:
- the secret is padded to match the expected block size of the hash function
- the padded key is then XORed with a constant (usually a block of zeros and ones)
- the message is hashed using the hash function with the XORed key at the beggining
- The result is hashed again with the same process but this time with another constant used for the XOR
- the final output us the HMAX value, a fixed-size string

The mathematical expression is:

_HMAC_(_K_,_M_) = _H_( (_K ⊕ opad_) || _H_( (_K ⊕ ipad_) || _M_) )
