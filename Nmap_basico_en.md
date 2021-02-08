# Nmap - Basic

## TOPICS

* [What is Nmap?](#What-is-Nmap?)
* [Features](#Features)
* [Installation](#Installation)
     * [Windows](#Windows)
     * [Linux](#Linux)
     * [Mac OS](#Mac-OS)
* [Port scanning](#Port-scanning)
* [List of all basic commands](#List-of-all-basic-commands)
     * [Select objectives](#Select-objectives)
     * [Discover systems](#Discover-systems)
     * [Port analysis techniques](#Port-analysis-techniques)
     * [Ports to analyze and order of analysis](#Ports-to-analyze-and-order-of-analysis)
     * [Duration and execution](#Duration-and-execution)
     * [Service and version detection](#Service-and-version-detection)
     * [Bypassing Firewalls/IDS](#Bypassing-Firewalls/IDS)
     * [Other options](#Other-options)
* [Command examples](#Command-examples)


## What is Nmap?

Nmap, short for Network Mapper, is a free, open source tool for vulnerability scanning and network detection.
Network administrators use Nmap to identify which devices are running on their systems,
discover available hosts and the services they offer, find open ports, and detect security risks.

Nmap can be used to monitor individual hosts as well as large networks spanning hundreds of thousands of devices and multitudes of subnets.


## Characteristics

Nmap is one of the best host discovery and port scanning tools out there. This allows us to obtain
a large amount of information about the computers on our network, it is capable of scanning hosts that are up, checking if they have any open ports,
if they are filtering ports (they have a firewall enabled), and even knowing what operating system a certain target is using.

Nmap allows us to detect hosts on a local network, and also through the Internet, in this way,
We will be able to know if these hosts (computers, servers, routers, switches, IoT devices) are currently connected to the Internet or to the local network.
This tool also allows us to perform a port scan to the different hosts, to see what services we have active in said hosts thanks to the fact that it will tell us the status of their ports,
we will be able to know which operating system a certain computer is using, and we will even be able to automate different pentesting tests to check the security of the computers.

## Installation

Nmap is a very flexible system, thanks to this this tool can be installed on a large number of devices, as we can see on its main page:
https://nmap.org/download.html

### Windows

To install the tool on windows systems we have to follow the following steps:

```
1 - download the following file: https://nmap.org/dist/nmap-7.91-win32.zip
2 - Unzip the file
3 - Execute the file nmap.exe from the terminal (example: "CMD")
```

### Linux

In linux we have the opportunity to install from the source code or from the packages, if we install from the packages the commands will change depending on the distribution.

Examples:

Installing from RPMs

```
1- Installing Nmap from binary RPMs

rpm -vhU https://nmap.org/dist/nmap-4.68-1.i386.rpm

2- Building and installing Nmap from source RPMs

rpmbuild --rebuild https://nmap.org/dist/nmap-4.68-1.src.rpm
```
```
Installing Nmap from a system Yum repository

yum install nmap

Installing Nmap from a system Apt repository

apt-get install nmap
```
https://nmap.org/book/inst-linux.html#inst-rpm 

### Mac OS

steps to install nmap on mac:

```
1 - download the following file: https://nmap.org/dist/nmap-7.91.dmg
2 - install.
```
https://nmap.org/book/inst-macosx.html

## Port scanning

The packets that Nmap sends return IP addresses and a great deal of other data, allowing you to identify all kinds of network attributes, creating a network map or profile, and creating an inventory of hardware and software.

Different protocols use different types of packet structures. Nmap uses transport layer protocols including TCP (Transmission Control Protocol), UDP (User Datagram Protocol), and SCTP (Stream Control Transmission Protocol), as well as supporting protocols such as ICMP (Internet Control Message Protocol), used to send messages from error.

Different protocols serve different purposes and ports on the system. For example, UDP resource overhead is suitable for streaming video, where some lost packets are sacrificed for speed, while streaming videos on YouTube are buffered and they use the slower, but more reliable TCP protocol.

## List of all basic commands

This program is really complete, so far we have used the basic commands to discover hosts and also to see if it has open ports, however, this does not stay that way, and we have a large list of commands to make the most of this tool.

### Select objectives

IP addresses or ranges, names of systems, networks, etc.

Example: scanme.nmap.org

-iL file list in file -iR n choose targets randomly, 0 never ends
–Exclude –excludefile file exclude systems from file

### Discover systems
```
-PS n tcp syn ping
-PA n ping TCP ACK
-PU n ping UDP
-PM Netmask Req
-PP Timestamp Req
-PE Echo Req
-sL list analysis
-PO ping per protocol
-PN Do not ping
-n don't do DNS
-R Resolve DNS on all target systems
–Traceroute: trace route to system (for network topologies)
-sP ping, same as –PP –PM –PS443 –PA80
```
### Port analysis techniques
```
-sS parse using TCP SYN
-sT parsing using TCP CONNECT
-sU analysis using UDP
-s AND analysis using SCTP INIT
-sZ using COOKIE ECHO from SCTP
-sO IP protocol
-sW TCP window -sN
–SF -sX NULL, FIN, XMAS
–SA TCP ACK
```
### Ports to analyze and order of analysis
```
-p n-mrango
-p– all ports
-p n, m, z specified
-p U: n-m, z T: n, m U for UDP, T for TCP
-F fast, the common 100
–Top-ports n analyze the most used ports
-r not random
```
### Duration and execution
```
-T0 paranoid
-T1 stealth
-T2 sophisticated
-T3 normal
-T4 aggressive
-T5 madness
–Min-hostgroup
–Max-hostgroup
–Min-rate
–Max-rate
–Min-parallelism
–Max-parallelism
–Min-rtt-timeout
–Max-rtt-timeout
–Initial-rtt-timeout
–Max-retries
–Host-timeout –scan-delay
```
### Service and version detection
```
-sV: services version detection
–All-ports do not exclude ports
–Version-all test each scan
–Version-trace trace version analysis activity
-Or activate detection of the Operating System
–Fuzzy guess OS detection
–Max-os-tries set maximum number of attempts against target system
```

### Firewall / IDS Evasion

```
-f fragment packets
-D d1, d2 cloak analysis with decoys
-S ip spoof source address
–G source spoof source port
–Randomize-hosts order
–Spoof-mac mac change source MAC
Level of Detail and Debugging Parameters
-v Increase the level of detail
–Reason reasons by system and port
-d (1-9) set debugging level
–Packet-trace packet path
```

### Other options

```
–Resume file continue analysis aborted (taking output formats with -oN or -oG)
-6 enable IPV6 scanning
-A aggressive, same as -O -sV -sC –traceroute
Interactive options
v / V increase / decrease level of analysis detail
d / D increase / decrease debugging level
p / P enable / disable packet trace
Scripts
-sC perform analysis with default scripts
–Script file run script (or all)
–Script-args n = v provide arguments
–Script-trace show incoming and outgoing communication
Output formats
-oN save in normal format
-oX save in XML format
-oG save in format to later use Grep
-oA save in all previous formats
```

These are mainly the commands available to Nmap. Before finishing, we must say that Nmap has a multitude of options with which to perform complete network analysis. We can consult all the available options by typing:

nmap --help


## Command examples

For all the examples that we are going to see, we will use the page http://scanme.nmap.org/

#### Scan by hostname

`nmap scanme.nmap.org`

#### Scan using the "-v" option

You can see that the following command with the option "-v" and more detailed information about the remote machine.

`nmap -v scanme.nmap.org`


#### Parse the list of hosts in a file

`nmap -iL nmaptest.txt`

#### Scan OS and Traceroute Information

With Nmap, you can detect what operating system and version you are running on the remote host. To enable OS and Version Detection, Write Scan, and Trace Path, we can use the “-A” option with NMAP.

`nmap -A scanme.nmap.org`

#### Enable OS Discovery with Nmap

Use the option "-O" and "-osscan-guess" also helps to discover the operating system information.
 
`nmap -O scanme.nmap.org`

#### Scan a Host for Firewall Detection

The following command will search a remote host to detect whether packet filters or Firewall is used by the host.

`nmap -sA scanme.nmap.org`

#### Scan a host to check its firewall protection

`nmap -PN scanme.nmap.org`

#### Find Version Host Services Numbers

We can find versions of services that run on remote machines with the "-sV" option.

`nmap -sV scanme.nmap.org`

#### Scan remote hosts via TCP ACK (PA) and TCP Syn (PS)

Sometimes packet filtering firewalls block standard ICMP ping requests, in that case we can use TCP ACK and TCP Syn methods to scan remote hosts.

`nmap -PS scanme.nmap.org`

#### FIN scan of Nmap

Aggressive FIN scan

`nmap -sF -T4 scanme.nmap.org`

Insanely aggressive FIN-type scan against a device.

`nmap -sF -T5 scanme.nmap.org`

------------------------------

I hope this writing is to your liking, it was designed to introduce us to the topic of NMAP scans.

