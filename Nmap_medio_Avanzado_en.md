# Nmap - Medium - Advanced

## TOPICS

* [Introduction to TCP/IP](#Introduction-to-TCP/IP)
    * [TCP/IP]#(TCP/IP)
    * [Transmission Control Protocol (TCP)](#Transmission-Control-Protocol-(TCP))
    * [State of the ports](#State-of-the-ports)
    * [Flags](#Flags)
         * [SYN](#SYN)
         * [ACK](#ACK)
         * [RST](#RST)
         * [PSH](#PSH)
         * [URG](#URG)
         * [END](#END)
* [List of all basic commands](#List-of-all-basic-commands)
    * [Select objectives](#Select-objectives)
    * [Discover systems](#Discover-systems)
    * [Port analysis techniques](#Port-analysis-techniques)
    * [Ports to analyze and order of analysis](#Ports-to-analyze-and-order-of-analysis)
    * [Duration and execution](#Duration-and-execution)
    * [Service and version detection](#Service-and-version-detection)
    * [Bypassing Firewalls / IDS](#Bypassing-Firewalls / IDS)
    * [Other options](#Other-options)
* [Examples of useful-commands](#Examples-of-useful-commands)
* [RECOMMENDED](#RECOMMENDED)
* [Nmap Scripting Engine](#Nmap-Scripting-Engine)
* [Useful scripts](#Useful-scripts)
* [XSS and SQLi?](#XSS-and-SQLi?)
    * [XSS](#XSS)
    * [SQLi](#SQLi)
* [VULSCAN](#VULSCAN)
    * [Installation](#Installation)
* [AUTOMATING](#AUTOMATING)
    * [NSE-NMAP](#NSE-NMAP)
    * [Vulmap](#Vulmap)
    * [Brute-map](#Brute-map)
* [Acknowledgments](#Acknowledgments)

This is the continuation of the writing "Nmap - Basic" where only the characteristics of this tool were discussed, the use that is given to it, and we put aside the "practical" part, focusing the writing in a more "theoretical way. "and that way to be able to know" basic "so as not to get lost among the options that this tool allows us to use.

In this continuation, we will talk about a more practical point of view and we will do everything possible to gain as much value as possible among the options that this tool gives us.

## Introduction to TCP / IP

To start this well, first we are going to clarify some terms that we need to know if we are going to get into the world of networks, scans ...

### TCP / IP

Transmission Control Protocol (TCP) and Internet Protocol (IP). Basically these 2 make the basis for the entire internet. TCP is in charge of controlling, ordering, managing the data and making sure that it reaches its destination without packets being lost. It belongs to the Transport layer of the TCP / IP protocol.

IP takes care of sending the packets to their destination. It belongs to the Internet layer of TCP / IP.

### Transmission Control Protocol (TCP)

TCP acts as both a sender and a receiver of packets. The sending TCP divides the information to be transferred into packets and sends each one separately, retransmitting them if one is not delivered correctly. If necessary, the receiving TCP will have to reorder the packets sequentially (each packet has a 32-bit sequence number, which is a field in the header) in order to obtain the information.

Other fields in the header are Source port and Destination port, which are 16-bit unsigned numbers. TCP uses port numbers to identify the sending application and the receiving application. There are TCP ports from 0 to 65535, however we can distinguish three types of ports:
Known ports: From 0 to 1023, they are reserved by the IANA for certain types of applications (HTTP server, FTP, etc.) and administrator privileges are required on a machine to activate an application on one of these ports.

An example is port 80 (HTTP).

Registered ports: 1024 to 49151, reserved for specific applications. An example is 3306 (MySQL).
Private ports: from 49152 to 65535, these are not reserved for any specific application.

#### Port status

We can say that a port can be in three states:

```
Filtered port: A firewall (firewall) blocks access to the port.

Port closed: The port is not blocked but there is no application listening on it.

Port open: The port is not blocked and an application is listening on it.
```

The technique of detecting the open ports of a machine is called Port scanning, which is done for security purposes but also provides a lot of useful information to a possible attacker of our machine, since it allows you to know what applications we have active to exploit a bug and get access.

### Flags

In the header of the TCP packets there are 6 1-bit flags, that is, they can be 0 or 1 depending on whether they are deactivated or activated.
These flags are: SYN, ACK, RST, PSH, URG, and FIN.


#### SYN

SYN (Synchronize): used to initiate a TCP connection. If we send a packet with this flag to a port X of a machine, several things can happen:

If the port is open, the machine responds with a packet with the SYN and ACK flags.
If the port is closed, it responds with a packet with the RST and ACK flags.
If the port is filtered, we will not receive anything.

#### ACK

ACK (Acknowledgment): This flag is used for confirmations.

If we send an "unsolicited" ACK packet to port X of a machine, it should be answered with another packet with the RST flag, whatever the state of the port. In fact, there are firewalls that only filter packets attempting to connect with SYN, not filtering those with ACKs.

#### RST

RST (Reset): This bit is used to reset a connection due to corrupted packets or duplicate SYNs, delays, etc.

#### PSH

PSH (Push): It is used to force the immediate sending of the data as soon as possible. If the TCP
sender sends a packet with this flag activated, the receiving TCP knows that it has to deliver the data
immediately to the receiving application without putting it in a buffer and waiting for more data.

#### URG

URG (Urgent): Used to define a block of data as "urgent". The Urgent Pointer field of the headland is
a pointer that "points" to where this urgent data ends; in this way the data block is marked.

#### END

FIN (Finalize): It is used to end a connection.











