# Public key encryption (asymmetric encryption)

### Generating RSA public/private key pairs with `gpg`

The `gpg` program is the OpenPGP encryption and signing tool. PGP stands for "pretty good protection". 

__Step 1.__ Generate a public private key pair.

* First ssh to the Kali Linux machine. 

* Run the following `gpg` command:
    ```
    $ gpg --full-generate-key
    ```

* Follow the prompts to answer the questions. 

* Below is how I answered these questions to generate my keys:

    ```
    gpg (GnuPG) 2.2.40; Copyright (C) 2022 g10 Code GmbH
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    gpg: directory '/home/kali/.gnupg' created
    gpg: keybox '/home/kali/.gnupg/pubring.kbx' created
    Please select what kind of key you want:
    (1) RSA and RSA (default)
    (2) DSA and Elgamal
    (3) DSA (sign only)
    (4) RSA (sign only)
    (14) Existing key from card
    Your selection? 1
    RSA keys may be between 1024 and 4096 bits long.
    What keysize do you want? (3072)
    Requested keysize is 3072 bits
    Please specify how long the key should be valid.
            0 = key does not expire
        <n>  = key expires in n days
        <n>w = key expires in n weeks
        <n>m = key expires in n months
        <n>y = key expires in n years
    Key is valid for? (0)
    Key does not expire at all
    Is this correct? (y/N) y

    GnuPG needs to construct a user ID to identify your key.

    Real name: Dr. David Balash
    Email address: david.balash@richmond.edu
    Comment: RSA keys for CMSC 240
    You selected this USER-ID:
        "Dr. David Balash (RSA keys for CMSC 240) <david.balash@richmond.edu>"

    Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    gpg: /home/kali/.gnupg/trustdb.gpg: trustdb created
    gpg: directory '/home/kali/.gnupg/openpgp-revocs.d' created
    gpg: revocation certificate stored as '/home/kali/.gnupg/openpgp-revocs.d/3EE0A0EFB95B62BECAE0F893059FA9C60FCE1D9B.rev'
    public and secret key created and signed.

    pub   rsa3072 2024-01-31 [SC]
        3EE0A0EFB95B62BECAE0F893059FA9C60FCE1D9B
    uid                      Dr. David Balash (RSA keys for CMSC 240) <david.balash@richmond.edu>
    sub   rsa3072 2024-01-31 [E]
    ```

__Step 2.__ Verify that your keys where created.

* Run the following `gpg` command:
    ```
    $ gpg --list-public-keys
    ```
* Mine looks like this:
    ```
    gpg: checking the trustdb
    gpg: marginals needed: 3  completes needed: 1  trust model: pgp
    gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
    /home/kali/.gnupg/pubring.kbx
    -----------------------------
    pub   rsa3072 2024-01-31 [SC]
        3EE0A0EFB95B62BECAE0F893059FA9C60FCE1D9B
    uid           [ultimate] Dr. David Balash (RSA keys for CMSC 240) <david.balash@richmond.edu>
    sub   rsa3072 2024-01-31 [E]
    ```

__Step 3.__ Output your public key to a file.

* Run this `gpg` command to output your public key to a file (but __change__ the email address to your email address and __change__ the name of your key to your firstname.key):
    ```
    $ gpg --output ~/firstname.key --armor --export your.email@richmond.edu
    ```

__Step 4.__ Take a look at your key. 
    ```
    $ cat firstname.key
    ```
* Here is what mine looks like:
    ```
    -----BEGIN PGP PUBLIC KEY BLOCK-----

    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
    =wZCC
    -----END PGP PUBLIC KEY BLOCK-----
    ```

__Step 5.__ Copy your key to the directory `/opt/publickeys/`.
    ```
    $ cp firstname.key /opt/publickeys/
    ```




