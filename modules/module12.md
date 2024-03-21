# Module 12: Packet Sniffing and Spoofing

* Put your answers in the `README.md` file in the GitHub repository.
* Github Classroom Link: [https://classroom.github.com/a/5EFWaPUt](https://classroom.github.com/a/5EFWaPUt)

## Overview

Packet sniffing and spoofing are two important concepts in network security; they are two major threats in network communication. Being able to understand these two threats is essential for understanding security measures in networking. There are many packet sniffing and spoofing tools, such as `Wireshark`, `tcpdump`, `Scapy`, etc. Some of these tools are widely used by security experts, as well as by attackers. Being able to use these tools is important for students, but what is more important for students in a network security course is to understand how these tools work, i.e., how packet sniffing and spoofing are implemented in software. The objective of this module is two-fold: learning to use the tools and understanding the technologies underlying these tools. For the second object, students will write simple sniffer and spoofing programs, and gain an in-depth understanding of the technical aspects of these programs. This module covers the following topics:

- How the sniffing and spoofing work
- Packet sniffing and spoofing using the `Scapy` Python library
- Manipulating packets using the `Scapy` Python library


## Setup

Add the following to your .ssh/config file on your local machine, and replace `[YOUR NET ID]` with your UR netid. 
```
Host cmsc334-1
HostName x-mcs-cmsc334-1.richmond.edu
User [YOUR NET ID]
IdentityFile ~/.ssh/id_rsa_cmsc334

Host cmsc334-2
HostName x-mcs-cmsc334-2.richmond.edu
User [YOUR NET ID]
IdentityFile ~/.ssh/id_rsa_cmsc334

Host cmsc334-3
HostName x-mcs-cmsc334-3.richmond.edu
User [YOUR NET ID]
IdentityFile ~/.ssh/id_rsa_cmsc334
```


You should now be able to ssh directly to the Kali linux machines that we will use for this module. 

```shell
$ ssh cmsc334-1
```


After you login to `cmsc334-1` run interface configure command `ifconfig` to list the active network interfaces and take note of the ethernet network interface and IPv4 address.

For example, when running the `ifconfig` command below, the network interface is `eth0`, and the IPv4 address is `172.18.123.6`. The interface `lo` is the loopback interface which is always `127.0.0.1` and is used for testing purposes.
```shell
$ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.18.123.6  netmask 255.255.254.0  broadcast 172.18.123.255
        inet6 fe80::ea6a:64ff:fece:4bdf  prefixlen 64  scopeid 0x20<link>
        ether e8:6a:64:ce:4b:df  txqueuelen 1000  (Ethernet)
        RX packets 174589  bytes 49208123 (46.9 MiB)
        RX errors 0  dropped 1  overruns 0  frame 0
        TX packets 96386  bytes 21776671 (20.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 17  memory 0xb1100000-b1120000

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 4  bytes 200 (200.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4  bytes 200 (200.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```




## Using `Scapy` to Sniff and Spoof Packets

`Scapy` is a packet manipulation library written in Python. It can forge or decode packets, send them on the wire, capture them, and match requests and replies. It can also handle tasks like scanning, tracerouting, probing, unit tests, attacks, and network discovery. Website: [https://scapy.net/](https://scapy.net/)

Many tools can be used to do sniffing and spoofing, but most of them only provide fixed functionalities. `Scapy` is different: it can be used not only as a tool, but also as a building block to construct other sniffing and spoofing tools, i.e., we can integrate the `Scapy` functionalities into our own program. In this set of tasks, we will use `Scapy` for each task. To use `Scapy`, we can write a Python program, and then execute this program using Python. See the following example. We should run Python using the `root` privilege because the privilege is required for spoofing packets. At the beginning of the program , we should import all `Scapy`'s modules.


### Task 1: Sniffing Packets

Wireshark is the most popular sniffing tool, and it is easy to use. However, it is difficult to use Wireshark as a building block to construct other tools. We will use Scapy for that purpose. The objective of this task is to learn how to use Scapy to do packet sniffing in Python programs. Add the following sample code to a file called `sniffer.py`:

__sniffer.py__
```Python
#!/usr/bin/python
from scapy.all import *

def print_pkt(pkt):
    pkt.show()

pkt = sniff(iface='eth0', filter='icmp', prn=print_pkt)
```

The code above will sniff the packets on the `eth0` interface. Please read the instruction in the setup section regarding how to get the interface name. It will most likely by `eth0` and so no change will be necessary. 

Note that the `filter` is set to filter `icmp` packets. ICMP stand for Internet Control Message Protocol. These packets are used by the diagnostic tools `ping` and `traceroute`.  

Become root user to run the script.
```shell
$ sudo su
#
```

Make sure that the Python script is executable.
```shell
# chmod u+x sniffer.py
```

Run the sniffer and see what you detect.
```shell
# ./sniffer.py
```

If you are not detecting any packets. Open a new terminal window and `ssh` to `cmsc334-2`.  From there you should `ping` `cmsc334-1`.  

From __new terminal__ window ssh to cmsc334-__`2`__
```shell
$ ssh cmsc334-2
```

In the new terminal ping the host that is sniffing `icmp` packets.
```shell
$ ping cmsc334-1
```

* In the terminal window with the sniffer what do you see? Add this to your __README.md__ file.

Now stop the `ping` command by typing `Ctrl-C` in the `cmsc334-2` terminal window, and stop the sniffer by typing `Ctrl-C` in the `cmsc334-1` terminal window.


#### Sniffing TCP Packets
Next, lets change the filter in the Python `sniffer.py` code. Modify the filter to capture tcp packets on a specific port number.  __Pick a random port between 1000 and 5000.__ Replace port `1234` below with your random port number choice.

__sniffer.py__
```Python
#!/usr/bin/python
from scapy.all import *

def print_pkt(pkt):
    pkt.show()

pkt = sniff(iface='eth0', filter='tcp port 1234', prn=print_pkt)
```

Run the new version of the sniffer program.
```shell
# ./sniffer.py
```

You will probably not detect any activity on this random port, so lets create some activity. Open another new terminal window. In this terminal window ssh again to `cmsc334-1`.

```shell
$ ssh cmsc334-1
```

Now we are going to use the `nc` command, the "_netcat_" utility, to listen for TCP packets on the random port that you selected above. For example, if you selected 1234.

In the __new__ `cmsc334-1` terminal window, run the `nc` command with the `-l` option to listen and the `-p` option to select a port. 
```shell
$ nc -l -p 1234
```

In another __new__ terminal window ssh to `cmsc334-`__`2`__. 

```shell
$ ssh cmsc334-2
```

In this __new__ `cmsc334-`__`2`__ terminal. Run the `nc` command to talk to the listening `netcat` on `cmsc334-`__`1`__. To do this list the host you would like to connect to and the port number. After you hit enter, directly below this you can now enter the message you would like to send and hit enter.
```shell
$ nc x-mcs-cmsc334-1 1234
Hello world... <enter>
```

Check your other terminal windows. In the terminal with the listening `netcat` you should see your message. And in the terminal window with your packet sniffer you should see the information for a TCP packet, including your message in the load. 

* Take note of the source (`src`) and destination (`dest`) IP addresses in the packet that you captured. They should match the ip addresses of `cmsc334-2` and `cmsc334-1`. Add this to your __README.md__ file along with a copy of the packet that you captured. 

* Modify the `sniffer.py` code to capture any TCP packet that comes from port number 22 (`ssh`). Test this by opening a new terminal window and ssh'ing to `cmsc334-1`. Copy and paste the captured packet results into your __README.md__ file. Put your `sniffer.py` code into the GitHub repository. 



### Task 2: Spoofing Packets

As a packet spoofing tool, Scapy allows us to set the fields of IP packets to arbitrary values. The objective of this task is to spoof IP packets with an arbitrary source IP address. We will spoof `ICMP echo request` packets, and send them to another VM on the same network. We will use our __sniffer.py__ to observe the packet. If it is accepted, an `ICMP echo reply` packet will be sent to the spoofed IP address. The following code shows an example of how to spoof an ICMP packets.

__spoof.py__
```Python
#!/usr/bin/python
from scapy.all import *

# Create an IP packet and set the destination.
ip = IP()
# Set the destination address to be cmsc334-2
ip.dst = '172.18.123.5'  
# Spoof the source address to be cmsc334-3
ip.src = '172.18.122.11'

# Create an ICMP packet.
icmp  = ICMP()

# Stack the layers by encapsulating the ICMP packet in the IP packet.
packet = ip/icmp

# Send the packet.
send(packet)
```

Before running your program.  Put a version of your sniffer program set to filter for ICMP packets on both `cmsc334-2` and `cmsc334-3`.

__sniffer.py__
```Python
#!/usr/bin/python

from scapy.all import *

def print_pkt(pkt):
    pkt.show()

pkt = sniff(iface='eth0', filter='icmp', prn=print_pkt)
```

Run the sniffer program on both both `cmsc334-2` and `cmsc334-3` as the root user.
```shell
$ chmod u+x sniffer.py
$ sudo su
# ./sniffer.py
```

Finally, run the spoof.py Python program as `root`.
```shell
# ./spoof.py
```

* Did the echo request go to `cmsc334-2`, and the echo reply go to the spoofed address `cmsc334-3`? 
* Cut and paste the packets that you captured from the sniffer programs into your `README.md` file. 
























