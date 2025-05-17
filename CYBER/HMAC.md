- HMAC: Keyed-Hash Message Authentication Code
- uses cryptographic hash with secret key to verify authenticity/integrity of data
![[Pasted image 20250311012213.png]]
Steps:
1. The secret key is padded to the block size of the hash function.
2. The padded key is XORed with a constant (usually a block of zeros or ones).
3. The message is hashed using the hash function with the XORed key.
4. The result from Step 3 is then hashed again with the same hash function but using the padded key XORed with another constant.
5. The final output is the HMAC value, typically a fixed-size string.


