# Module 4: Applied cryptography

* Put your answers in the README.md file in the GitHub repository.
* Github Classroom Link: [https://classroom.github.com/a/W7S2B0pW](https://classroom.github.com/a/W7S2B0pW)

### Exercise 1 - Diffie-Hellman key exchange

With Diffie-Hellman key exchange, two parties arrive at a common secret key, without passing the common secret key in public.

Complete the following steps to exchange a private key with a partner.

__Step 1.__ Find a partner to exchange keys with.

__Step 2.__ Choose one person to represent Alice and the other person to represent Bob. 

__Step 2.__ Agree upon the public key values of `g` and `n`. Use a _prime number_ for `n`.

__Step 3.__ Alice chooses a number for private key `a` and Bob chooses a number for private key `b`.

__Step 4a.__ Alice calculates the value `A`:
$$A = g^a \mod n$$

__Step 4b.__ Bob calculates the value `B`:
$$B = g^b \mod n$$

__Step 5.__ Alice and Bob exchange values `A` and `B`.

__Step 6a.__ Alice calculates a copy of the shared secret key using Bob's public `B` and Alice's private `a`:
$$key = B^a \mod n$$

__Step 6b.__ Bob calculates a copy of the shared secret key using Alice's public `A` and Bob's private `a`:
$$key = A^b \mod n$$

__Step 7.__ Verify with your partner that you now have the same shared secret key.

__Step 8.__ Record your math and results in the `README.md` file in your GitHub repository for this module. 


### Exercise 2 - Encrypt a file using a partner's public key (Asymmetric cryptography)

The goal of this exercise is to use a partner's public key to encrypt a message that only they can decrypt using their private key.

__Step 1.__ Find a partner to exchange encrypted messages with.

__Step 2.__ Change directory to where the public keys are stored for this class. And list available the keys.
```
$ cd /opt/publickeys/
$ ls
```

__Step 3.__ Import your partner's public key file. 
```
gpg --import name_of_pub_key_file
```

__Step 4.__ List your public keys.
```
$ gpg --list-public-keys
```

__Step 5.__ Sign your partner's key. Signing a key tells the `gpg` software that you trust the key that you have been provided with and that you have verified that it is associated with the person in question.
```
$ gpg --sign-key your.partner@richmond.edu
```

__Step 6.__ Change directory back to your home directory and create a plaintext message. Edit a plaintext file and write a short message for your partner (maybe your favorite flavor of ice cream).
```
$ cd ~
$ nano partnersname.txt
```

__Step 7.__ Encrypt the plaintext message using your partner's _public_ key. 
```
$ gpg --encrypt --armor --recipient your.partner@richmond.edu partnersname.txt
```

__Step 8.__ View your encrypted file.
```
$ cat partnersname.txt.asc
```

__Step 9.__ Copy the __encrypted__ file to the `/opt/messages/` directory.
```
$ cp partnersname.txt.asc /opt/messages/
```

__Step 10.__ Copy the encrypted file that your partner left for you in the `/opt/messages` directory to your home directory.
```
$ cd /opt/messages/
$ ls 
$ cp yourname.txt.asc ~
```

__Step 11.__ Change directory back to your home directory and decrypt the file.
```
$ cd ~
$ gpg --decrypt yourname.txt.asc
```

__Step 12.__ Put the results of the `gpg` decrypt command into the `README.md` file in your GitHub repository for this module. 

__Step 13.__ In the `README.md` file explain who has access to decrypt the encrypted file. Attempt to decrypt the `partnersname.txt.asc` with your private key. Explain the results of this command and why this is the case.
```
$ gpg --decrypt partnersname.txt.asc
```

__Step 14.__ Re-encrypt the partnersname.txt file but this time use __both__ your partner's and your public key.
```
$ rm partnersname.txt.asc
$ gpg --encrypt --armor --recipient your.partner@richmond.edu --recipient your.email@richmond.edu partnersname.txt
```




### Exercise 3 -  Sign a file using your private key (Asymmetric cryptography)

With a digital signature, a sender can use a _private key_ together with a message to create a signature. Anyone with the corresponding _public key_ can verify whether the signature matches the message, but a forger who does not know the _private key_ cannot sign a message as the own of the _private key_.

__Step 1.__ Create a new plaintext message in a new file.
```
$ nano signedbyyourname.txt
```

__Step 2.__ Sign the plaintext file with your private key.
```
$ gpg --sign --armor signedbyyourname.txt
```

__Step 3.__ View your encrypted file.
```
$ cat signedbyyourname.txt.asc
```

__Step 4.__ Copy your signed file to the `/opt/signed/` directory.
```
$ cp signedbyyourname.txt.asc /opt/signed/
```

__Step 5.__ Copy the encrypted file that your partner left for you in the `/opt/messages` directory to your home directory.
```
$ cd /opt/messages/
$ ls 
$ cp signedbypartner.txt.asc ~
```

__Step 6.__ Change directory back to your home directory. Then decrypt and read your partner's signed file.
```
$ cd ~
$ gpg --decrypt signedbypartner.txt.asc
```

__Step 7.__ Put the results of the `gpg` decrypt command into the `README.md` file in your GitHub repository for this module.

__Step 8.__ In the `README.md` file explain who has access to decrypt the file and why.


### Exercise 4 - Encrypt a file using a partner's public key and sign the file using your private key (Asymmetric cryptography)

__Step 1.__ Use the `gpg` program to encrypt using your partner's public key, but also sign the message with your private key. 

Hint: You can use both the `--encrypt` and `--sign` options in a single command.

* Example
```
$ gpg --encrypt --sign --armor --recipient your.partner@richmond.edu partnersname.txt
```

__Step 2.__ Experiment with the `gpg` command in step 1.  In the `README.md` file, write down the steps you took to share an encrypted and signed message. 

__Step 3.__ In the `README.md` file explain who has access to decrypt the file and why.


### Exercise 5 - Encrypt a file using a shared secret key with AES (Symmetric cryptography)

Symmetric-key algorithms are algorithms for cryptography that use the same cryptographic keys for both the encryption of plaintext and the decryption of ciphertext. 

__Step 1.__ Find out what ciphers are available with the `gpg` program, and add this to the `README.md` file.
```
$ gpg --version
```

__Step 2.__ Create a new message to encrypt.
```
$ nano yournamesymmetric.txt
```

__Step 3.__ Think of a passphrase. Share this passphrase with your partner, this is your shared symmetric key.

__Step 4.__ Encrypt the file using `AES256`. When prompted for a passphrase enter the phrase from step 3. 
```
$ gpg --armor --symmetric --cipher-algo AES256 yournamesymmetric.txt
```

__Step 5.__ View your encrypted file.
```
$ cat yournamesymmetric.txt.asc
```

__Step 6.__ Copy your encrypted file to the `/opt/symmetric/` directory.
```
$ cp yournamesymmetric.txt.asc /opt/symmetric/
```

__Step 7.__ Copy your partner's encrypted file to your home directory.
```
$ cd /opt/symmetric/
$ ls
$ cp partnernamesymmetric.txt.asc ~
```

__Step 8.__ Change directory back to your home directory. Then decrypt and read your partner's signed file. 
```
$ cd ~
$ gpg --decrypt partnernamesymmetric.txt.asc
```

__Step 9.__ Put the results of the `gpg` decrypt command into the `README.md` file in your GitHub repository for this module.

__Step 10.__ In the `README.md` file explain who has access to decrypt the encrypted file.





