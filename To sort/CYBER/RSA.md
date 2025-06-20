RSA is based on the mathematical difficulty of factoring a large number. 

Multiplying two prime numbers is easy but finding out the decomposition in prime numbers of a number is difficult.

it is difficult for a computer to factorise a number with 600 digits for example.

The RSA algorithm goes as follows:
- Bob choses two prime numbers p, and q, he calculates n = p x q.
- Given _ϕ_(_n_) = _n_ − _p_ − q, he choses e, such that e is relatively prime to _ϕ_(_n_). He also selects d, such that e x d = 1 mod _ϕ_(_n_). The public key is (n, e) and the private key is (n, d).
- if Alice wants to encrypt a given integer x, she would send y = x^e mod n.
- Bob decrypts by calculating y^d mod n and retrieves x.

Some other usual notations in addition of the ones above are:
- "m" for the plaintext
- "c" for the ciphertext

