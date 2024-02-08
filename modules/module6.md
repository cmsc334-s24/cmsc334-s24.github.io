# Module 6: Linux Process Management and Bash Scripting

* Put your answers in the `README.md` file in the GitHub repository.
* Github Classroom Link: [https://classroom.github.com/a/nsVum9zy](https://classroom.github.com/a/nsVum9zy)


## Introduction

This module is designed to introduce the basics of process management in Linux and the fundamentals of bash scripting. Students will learn how to create and manage processes, and understand the role of basic bash scripting in automating tasks.

## Exercise 1: Creating Custom Bash Scripts

### Writing Your First Bash Script

#### Step 1: Script Basics

Before we start writing the script, it's important to understand the line `#!/bin/bash`. This line tells the system that this script should be run in the bash shell environment. It's placed at the top of the script file and is necessary for executing the script correctly.

#### Step 2: Create the Script File

1. **Create a Script File**: Open a text editor to create a file named `script1.sh`:

    ```bash
    nano script1.sh
    ```

2. **Add Content to the Script**: Enter the following content into `script1.sh`, which makes the script print a message and then sleep for a random duration between 1 and 5 seconds:

    ```bash
    #!/bin/bash

    # The following is a while loop that 
    # will echo a string forever. 
    while true
    do
        echo "Script 1 is running"
        sleep $((1 + RANDOM % 5))
    done
    ```

3. **Make the Script Executable**: After saving the file, change the script's permissions to make it executable:

    ```bash
    chmod +x script1.sh
    ```

4. **Run the Script**: 
    ```shell
    $ ./script1.sh
    ```

5. **Stop the script**: After it runs for a few seconds, type `Ctrl-C` to cancel the execution of the script. Control-C sends the SIGINT (Signal Interrupt) signal to the process running in the foreground of the terminal. This signal is intended to interrupt the process, causing it to terminate immediately.

#### Step 3: Repeat for a Second Script

1. Create `script2.sh` with a similar structure but a different message, making sure to also make it executable:

    ```bash
    nano script2.sh
    ```

    ```bash
    #!/bin/bash

    # The following is a while loop that 
    # will echo a string forever. 
    while true
    do
        echo "Script 2 is running"
        sleep $((1 + RANDOM % 5))
    done
    ```

    ```bash
    chmod +x script2.sh 
    ``` 

    ```shell
    $ ./script2.sh
    ```

    ```shell
    Ctrl-C 
    ```

## Exercise 2: Running and Managing Processes

Running the Scripts Concurrently. Using the `&` after a command signals to the Linux shell that you want to run the command in the background, and leave the shell ready for entering another command. 

1. Execute Both Scripts in Background:
    ```bash
    $ ./script1.sh >> output.txt &
    $ ./script2.sh >> output.txt &
    ```
2. Verify they are running by viewing the output file with the `tail` command.  The `tail` command with the `-f` flag allows you to monitor the file as more data is appended to it.
    ```bash
    $ tail -f output.txt
    ```
3. Make note of the pattern of "Script 1" versus "Script 2" being printed to the `output.txt` file. Put your observation in the `README.txt` file in the GitHub repository for this module. 
4. To stop the tail command type `Ctrl-C`.
5. Observing Process Scheduling. Monitor with jobs:
    ```bash
    $ jobs
    ```
    Use top or htop:
    ```bash
    $ top
    ```
    ```bash
    $ htop
    ``` 
6. Managing Processes. Suspending and Resuming. Bring a script to foreground.  Use the correct job numbers as listed by the jobs command. Replace `jobnumber` with the number of the job, probably `1` or `2`. But leave the `%` sign. Example: `fg %1`
    ```bash
    $ jobs
    $ fg %jobnumber
    ```
7. Then suspend it with `Ctrl+Z`.
8. Resume in the background with 
    ```bash
    $ jobs
    $ bg %jobnumber
    ```
9. Terminating Processes:
    ```bash
    $ jobs
    $ kill %jobnumber
    $ kill %jobnumber
    ```

## Exercise 3: Write and test your own bash script

1. Create a single bash script that will do the following.
* Create three new files with the touch command `touch file1.txt file2.txt file3.txt`
* Print "HelloBash" in each file 5 times with the `echo HelloBash >> file1.txt` command.
* Do a line count on each file with the `wc -l file.txt` command.
* Make a new directory called `filestore`.
* Move all three file to the new directory.

2. Test the script to make sure that it works.

3. Put this script in your GitHub repository for this module. 
