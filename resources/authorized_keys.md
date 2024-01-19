---
sort: 4
---

# ssh using authorized_keys

> You can use this guide to connect via `ssh` to Linux machines without using a password, but instead use an authorized public/private key pair.

1. Follow these steps to generate a ssh-key with `ssh-keygen`
   - Open a `local` terminal on **your laptop** and type
   ```
   ssh-keygen -t rsa -b 4096
   ```
   - **Hit enter to each of the questions**
   - No, you do not need a passphrase

2. Follow these steps to copy your **public** key
   - In the `local` terminal type
   ```
   cat .ssh/id_rsa.pub
   ```
   - This will print out your public key, select and copy it from the terminal
   - Make sure you copy the whole thing beginning with `ssh-rsa` ending with something like `dbalash@m1-mcs-dbalash`

3. Open a new terminal and login to a Linux machine via ssh.
   - In the `local` terminal type
   ```
   ssh username@machine.name
   ```
   - Enter your password when prompted. (__Note__: you will not see the password characters as you type them.)
   - If you see a message like this
   ```
   RSA key fingerprint is 96:a9:23:5c:cc:d1:0a:d4:70:22:93:e9:9e:1e:74:2f.
   Are you sure you want to continue connecting (yes/no)? yes
   ```
    - Type yes and hit return.

4. Change directory to the `.ssh` folder.
    - In the new `Linux` terminal type
    ```
    cd .ssh
    ```
    - Note, if the above command does not work because there is no `.ssh` folder then run the following command to make the directory.
    ```
    mkdir .ssh
    cd .ssh
    ```

5. Edit or create the file `authorized_keys` to add your newly created public key to the file. 
    - In the `Linux` terminal type
    ```
    nano authorized_keys
    ```
    - Paste your public key into the file.
    - Save and exit nano, by typing Ctrl-X, the type Y (for yes), and hit enter to accept the file name to save to is authorized_keys.

6. Logout of the Linux machine.
    - In the `Linux` terminal type
    ``` 
    exit
    ```

7. On your **personal laptop** edit the `.ssh/config` file.
    - In the `local` terminal type
    ```
    nano .ssh/config
    ```
    Note: if you are on a Windows PC and nano is not installed, you can try notepad.exe like this `notepad.exe .ssh/config`

    - Add the following lines to the file
    ```
        Host kali
        HostName 44.201.224.29
        User username
        IdentityFile ~/.ssh/id_rsa
    ```
    - **Replace username with your username**. For example, dbalash for me. 
    - Save and exit nano, by typing Ctrl-X, the type Y (for yes), and hit enter to accept the file name to save to is ./ssh/config.

8. Connect again Linux machine.
    - In a `local` terminal type
    ```
    ssh kali
    ```

9. Troubleshooting:  if it did not work, you can try the following.
    - Open a terminal on **your laptop** type the following to start the ssh-agent
    ```
        eval "$(ssh-agent -s)"
    ```
    - then use the ssh-add command
    ```
        ssh-add .ssh/id_rsa
    ```



