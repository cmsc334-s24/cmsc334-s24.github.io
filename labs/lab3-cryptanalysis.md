# Lab 3: Cryptanalysis

* First read this page then start working through the lab with the GitHub classroom link below. 
* The files that you need to complete this lab are also found in the GitHub repository.
* Put your answers in the README.md file in the GitHub repository.
* Github Classroom Link: [https://classroom.github.com/a/U3E915nJ](https://classroom.github.com/a/U3E915nJ)

### Objective

Learn about how to use _frequency analysis_ to crack a substitution cipher.

__Frequency Analysis__: This is the most classic method, exploiting the fact that in any given language, certain letters and combinations of letters occur with characteristic frequencies. By analyzing the frequency of letters in the ciphertext and comparing them to known frequencies in the target language, an attacker can start to guess which letters correspond to which in the cipher.

### Cryptanalysis and cipher text cracking

In this lab, you are going to write two small programs to perform cryptanalysis and cipher text cracking --- this will come in the form of a letter frequency program `freq` and a letter substitution program `sub`. You will use both programs to decrypt a secret message provided to you `cipher.txt` that was encrypted with a substitution cipher, where each letter is replaced with a different letter. 

Perhaps the most famous substitution cipher is the Caesar cipher, attributed to the Roman emperor Julius Caesar, where it was used for communicating with his generals in secret code. The cipher is rather simple, and describes shifting the alphabet by three letters such that `A->C` and `B->D` and `C->E` and so on; decryption requires shifting in the reverse direction. More advanced substitution cipher allow for arbitrary substitutions, not just by a shift, as long as those substations are consistent throughout the document. 

Unfortunately, any substitution cipher is trivially broken with some basic tools and analysis because of one fatal flaw. The distribution of letters of the plain text will be identical to that of the cipher text (under the substitution). Consider that English text (if we assume the input, plain-text is English) has a known letter distribution: if we see that `Q` is now the most frequent letter in the cipher text, then it was probably substituted with `E` to get pack to the plain text. 

Your task for this part of the lab is to write two programs: `freq` and `sub`. The `freq` program will take as input a text file and report the alpha letter frequencies of that file (ignoring non-alpha characters), and the `sub` program will print out the text of a file with letter substations based on a __key-file__, containing the substitution rules.  

### `freq`

 *  The frequency program should take one argument, `input.txt`, as a command line argument. It can be run like below:
    ```
    ./freq input.txt
    ```
 * It should output the count of each letter frequency as well as it's percentage, with two decimal points of precision's. For example, you are provided a file called `gatsby.txt` containing F. Scott Fitzgerald's *The Great Gatsby*. Running `freq` on that file produces the following output:
    ```
    $ ./freq gatsby.txt 
    A: 18163 (8.20%)
    B: 3470 (1.57%)
    C: 5157 (2.33%)
    D: 10435 (4.71%)
    E: 26971 (12.17%)
    F: 4584 (2.07%)
    G: 5320 (2.40%)
    H: 13186 (5.95%)
    I: 15281 (6.90%)
    J: 435 (0.20%)
    K: 2103 (0.95%)
    L: 8736 (3.94%)
    M: 5813 (2.62%)
    N: 15248 (6.88%)
    O: 17231 (7.78%)
    P: 3489 (1.57%)
    Q: 171 (0.08%)
    R: 12556 (5.67%)
    S: 13441 (6.07%)
    T: 20284 (9.16%)
    U: 6350 (2.87%)
    V: 2022 (0.91%)
    W: 5735 (2.59%)
    X: 318 (0.14%)
    Y: 4890 (2.21%)
    Z: 148 (0.07%)
    ```

 * `freq` should ignore all non-alpha characters in the input file, and should count frequencies of lower and upper-case equivalent. That is `a` and `A` both count the same, not for different frequencies. As you can see above, report your frequencies using upper-case. 

### Sub

 * The `sub` program takes two inputs, `key.txt`, and `cipher.txt`, and run with the following command line arguments.
    ```
    ./sub key.txt cipher.txt
    ```

 * The `key.txt` file is organized where each letter `A-Z` matches to a substitution, like `Q:S` (`Q` is substituted by `S`). The `no-op` `key.txt` file  that does no-substitutions, would be like (and is provided in the repo as `key.txt`). 
    ```
    A:A
    B:B
    C:C
    ...
    Y:Y
    Z:Z
    ```
 As part of your analysis, you'll change the right part (after the `:`) in `key.txt` to decrypt the secret message with substitutions. After you decrypt the key by using your `freq` for your analysis, you need to __manually update__ the `key.txt` with your decrypted key . For example:
    ```
    A:M
    B:F
    C:L
    D:E
    ...
    ```
 
 * To test your substitution program, it should produce the following output when run on the test input and test key. 
    ```
    $ ./sub test_encrypt_key.txt test_plain.txt
    ... FO CMR TKHMO PKMWOFWMU YMUJH – GQJ WMB SKMP FO MKQJBX GQJ VQK SMKAOC MR GQJ LQJBX MWKQRR OCH WQUX AQQBR QV NMTUMB LHOM; GQJ WMB UFH QB FO QB OCH LKFUUFMBO AMKLUH-RMBXHX LHMWCHR QV RMBOKMTFBJR Y, FBCMUFBT OCH CHMXG RHM YMPQJKR; GQJ WMB RUHHP JBXHK FO LHBHMOC OCH ROMKR SCFWC RCFBH RQ KHXUG QB OCH XHRHKO SQKUX QV EMEKMVQQB; JRH FO OQ RMFU M AFBF KMVO XQSB OCH RUQS CHMYG KFYHK AQOC; SHO FO VQK JRH FB CMBX-OQ-CMBX-WQALMO; SKMP FO KQJBX GQJK CHMX OQ SMKX QVV BQZFQJR VJAHR QK OQ MYQFX OCH TMDH QV OCH KMYHBQJR LJTLUMOOHK LHMRO QV OKMMU (M AFBXLQTTFBTUG ROJPFX MBFAMU, FO MRRJAHR OCMO FV GQJ WMB’O RHH FO, FO WMB’O RHH GQJ – XMVO MR M LJRC, LJO YHKG, YHKG KMYHBQJR); GQJ WMB SMYH GQJK OQSHU FB HAHKTHBWFHR MR M XFROKHRR RFTBMU, MBX QV WQJKRH XKG GQJKRHUV QVV SFOC FO FV FO ROFUU RHHAR OQ LH WUHMB HBQJTC. 
    $ ./sub test_decrypt_key.txt test_cipher.txt
    ... IT HAS GREAT PRACTICAL VALUE – YOU CAN WRAP IT AROUND YOU FOR WARMTH AS YOU BOUND ACROSS THE COLD MOONS OF JAGLAN BETA; YOU CAN LIE ON IT ON THE BRILLIANT MARBLE-SANDED BEACHES OF SANTRAGINUS V, INHALING THE HEADY SEA VAPOURS; YOU CAN SLEEP UNDER IT BENEATH THE STARS WHICH SHINE SO REDLY ON THE DESERT WORLD OF KAKRAFOON; USE IT TO SAIL A MINI RAFT DOWN THE SLOW HEAVY RIVER MOTH; WET IT FOR USE IN HAND-TO-HAND-COMBAT; WRAP IT ROUND YOUR HEAD TO WARD OFF NOXIOUS FUMES OR TO AVOID THE GAZE OF THE RAVENOUS BUGBLATTER BEAST OF TRAAL (A MINDBOGGINGLY STUPID ANIMAL, IT ASSUMES THAT IF YOU CAN’T SEE IT, IT CAN’T SEE YOU – DAFT AS A BUSH, BUT VERY, VERY RAVENOUS); YOU CAN WAVE YOUR TOWEL IN EMERGENCIES AS A DISTRESS SIGNAL, AND OF COURSE DRY YOURSELF OFF WITH IT IF IT STILL SEEMS TO BE CLEAN ENOUGH.
    ```
 
 
 Note, that your `sub` program can both encrypt and decrypt, depending on the key.
 
 
---
### Requirements


You should complete the following for this part of the lab
1. Write a `freq` program based on the specifications above. 
2. Write a `sub` program based on the specifications above
3. Use `freq` program and the example `gatsby.txt` for your letter distribution to determine the key for `cipher.txt`
4. Once you believe you've successfully decrypted the message, save it by running the following command:
   ```
   $ ./sub key.txt cipher.txt > plain.txt
   ```
5. This will save the output to a file called `plain.txt`. Add that file to your repo for submission. 


