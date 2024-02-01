# Applied cryptography

### Diffie-Hellman key exchange

__Step 1.__ Find a partner to exchange keys with.

__Step 2.__ Choose one person to represent Alice and the other person to represent Bob. 

__Step 2.__ Agree upon the public key values of `g` and `n`. Use a _prime number_ for `n`.

__Step 3.__ Alice chooses a number for private key `a` and Bob chooses a number for private key `b`.

__Step 4.__ Alice calculates the value `A`:
$$A = g^a \mod n$$