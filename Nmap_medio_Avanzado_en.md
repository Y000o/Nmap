# Nmap - Medium - Advanced

## TOPICS

* [Introduction to TCP/IP](#Introduction-to-TCP/IP)
    * [TCP/IP](#TCP/IP)
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
    * [Bypassing Firewalls/IDS](#Bypassing-Firewalls/IDS)
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

## List of all basic commands

We are going to review each section again but unlike the last post we are going to delve a little more into the operation of the scans and some examples.

### Select objectives

IP addresses or ranges, names of systems, networks, etc.

Example: scanme.nmap.org

```
-iL file list in file
-iR n choose targets randomly, 0 never ends
–Exclude –excludefile file exclude systems from file
```

### Discover systems

In this section, commands are used to discover the HOSTs that are within a network.

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

Let's see some examples:

The following are similar, since they have a similar behavior but the flag used changes:
```
-PS n tcp syn ping
-PA n ping TCP ACK
-PU n ping UDP
```

#### -PS

-PS n tcp syn ping


This option sends an empty TCP packet with the SYN flag set. The default destination port is 80. Alternate ports can be specified as a parameter. The syntax is the same as for -p except that port type specifiers such as T: are not allowed. Examples are -PS22 and -PS22-25,80,113,1050,35000.

The SYN frag suggests to the remote system that it is trying to establish a connection. Normally, the destination port will be closed and an RST (reset) packet will be sent. If the port is open, the destination will take the second step of a TCP three-way handshake by responding with a TCP SYN / ACK packet. The machine running Nmap then breaks the nascent connection by responding with an RST instead of sending an ACK packet that would complete the three-way handshake and establish a full connection. The RST packet is sent by the kernel of the machine running Nmap in response to the unexpected SYN / ACK, not by Nmap itself.

example: `nmap -PS scanme.nmap.org`

##### -PO

-PO ping per protocol


The IP protocol ping sends IP packets with the specified protocol number set in its IP header. The protocol list is in the same format as the port lists in the TCP, UDP, and SCTP host discovery options discussed earlier.


#### -PN

-Pn (No ping)


This option bypasses the host discovery stage entirely. Normally, Nmap uses this stage to determine the active machines for a heavier scan and to measure the speed of the network. By default, Nmap only performs intensive probing such as port scanning, version detection, or OS detection against hosts that are active. Disabling host discovery with -Pn causes Nmap to try the requested scan functions against each specified destination IP address.

#### -Traceroute

–Traceroute: trace route to system (for network topologies)

Personally I really like the output of this option, I don't know ... I feel good as a hacker with the screen like this.


Traceroute works by sending packets with a low TTL (time to live) in an attempt to get ICMP time-over messages from intermediate hops between the scanner and the destination host. Standard traceroute implementations start with a TTL of 1 and increase the TTL until the destination host is reached. The Nmap traceroute starts with a high TTL and then decreases the TTL until it reaches zero. Doing it the other way around allows Nmap to employ smart caching algorithms to speed up traces across multiple hosts. On average, Nmap sends 5-10 fewer packets per host, depending on network conditions.



### Port analysis techniques

This section documents the dozen or so port scanning techniques supported by Nmap. By default, Nmap performs a SYN scan, although it replaces a connect scan if the user does not have the proper privileges to send raw packets (requires root access on Unix).

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

#### -sS

-sS parse using TCP SYN

SYN analysis is the default and most popular analysis option for good reasons. It can be done quickly, scanning thousands of ports per second in a fast network unhindered by restrictive firewalls. It is also relatively discreet and stealth, as it never completes TCP connections. SYN scanning works against any supported TCP stack rather than relying on specific platform idiosyncrasies like FIN / NULL / Xmas, Maimon, and idle Nmap scans do. It also allows a clear and reliable differentiation between the open, closed and filtered states.

#### -sT

-sT parsing using TCP CONNECT

TCP connection scan is the default TCP scan type when SYN scan is not an option. This is the case when a user does not have raw packet privileges. Instead of writing raw packets as most other types of scanning do, Nmap requests the underlying operating system to establish a connection to the target machine and port by issuing the connection system call. This is the same high-level system call that web browsers, P2P clients, and most other network-enabled applications use to establish a connection. It is part of a programming interface known as the Berkeley Sockets API. Instead of reading raw packet responses, Nmap uses this API to obtain status information on each connection attempt.

#### -his

-sU analysis using UDP

While the most popular services on the Internet run on top of the TCP protocol, UDP services are widely implemented. DNS, SNMP, and DHCP (registered ports 53, 161/162, and 67/68) are three of the most common. Because UDP scanning is generally slower and more difficult than TCP, some security auditors ignore these ports. This is a mistake, since exploitable UDP services are quite common and attackers certainly do not ignore the entire protocol. Fortunately, Nmap can help inventory UDP ports.

#### -sY

-s AND analysis using SCTP INIT

SCTP is a relatively new alternative to the TCP and UDP protocols, combining most of the features of TCP and UDP, as well as adding new features like multi-homing and multi-streaming. It is mainly used for SS7 / SIGTRAN related services, but has the potential to be used for other applications as well. The SCTP INIT scan is the SCTP equivalent of a TCP SYN scan. It can be done quickly, scanning thousands of ports per second in a fast network unhindered by restrictive firewalls. Like SYN analysis, INIT analysis is relatively discreet and stealthy, as it never completes SCTP associations. It also allows a clear and reliable differentiation between the open, closed and filtered states.

#### -sZ

-sZ using COOKIE ECHO from SCTP

The SCTP COOKIE ECHO scan is a more advanced SCTP scan. It takes advantage of the fact that SCTP implementations must silently remove packets containing COOKIE ECHO fragments on open ports, but send an ABORT if the port is closed. The advantage of this type of scan is that a port scan is not as obvious as an INIT scan. Also, there may be stateless firewall rule sets that block INIT fragments, but not COOKIE ECHO fragments. Don't be fooled into thinking that this will make a port scan invisible; a good IDS will also be able to detect SCTP COOKIE ECHO scans. The downside is that the SCTP COOKIE ECHO scans can't differentiate between open and filtered ports, leaving you in the open state | filtered in both cases.

#### -sO

-sO IP protocol

The IP protocol scan allows you to determine which IP protocols (TCP, ICMP, IGMP, etc.) are supported by the target machines. Technically, this is not a port scan as it loops through IP protocol numbers rather than TCP or UDP port numbers. However, it still uses the -p option to select scanned protocol numbers, reports its results within normal port table format, and even uses the same underlying scan engine as true port scan methods. So it's close enough to a port scan that it belongs here.

#### -sW

-sW TCP window -sN

Window scanning is exactly the same as ACK scanning, except that it takes advantage of a certain systems implementation detail to differentiate open ports from closed ones, instead of always printing without filtering when an RST is returned. It does this by examining the TCP window field of the returned RST packets. In some systems, open ports use a positive window size (even for RST packets) while closed ports have a zero window. So instead of always listing a port as unfiltered when it receives an RST, the Windows scan lists the port as open or closed if the TCP window value on that reset is positive or zero, respectively.

#### –sF -sX NULL, FIN, XMAS

–SF -sX NULL, FIN, XMAS

These three types of scanning (even more are possible with the --scanflags option described in the next section) take advantage of a subtle loophole in the TCP RFC to differentiate between open and closed ports. Page 65 of RFC 793 says that "if the [destination] port status is CLOSED ... an incoming segment that does not contain an RST causes an RST to be sent in response." The next page then looks at packets sent to open ports with no SYN, RST, or ACK bits set, stating that: "it is unlikely it will get here, but if it does, remove the segment and return."

When scanning systems that comply with this RFC text, any packet that does not contain SYN, RST, or ACK bits will result in an RST returned if the port is closed and there will be no response if the port is open. As long as none of those three bits are included, any combination of the other three (FIN, PSH, and URG) is fine.


#### –sA

–SA TCP ACK

This scan is different from the others discussed so far in that it never determines open (or even open | filtered) ports. It is used to map firewall rule sets, determining whether they are stateful or not and which ports are filtered.



### Ports to analyze and order of analysis

Nmap offers options to specify which ports are scanned and whether the scan order is random or sequential. By default, Nmap scans the 1000 most common ports for each protocol.

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
-T5 insanity
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

#### -T

-T paranoid | sneaky | polite | normal | aggressive | insane (Set a timing template)

These templates allow the user to specify how aggressive they want to be, while letting Nmap choose the exact time values. The templates also make some minor speed adjustments for which there are currently no detailed control options. For example, -T4 prohibits the dynamic scan delay from exceeding 10 ms for TCP ports, and -T5 limits that value to 5 ms. Templates can be used in conjunction with drill-down controls, and the drill-down controls you specify will take precedence over the default timing template for that parameter. I recommend using -T4 when scanning reasonably modern and reliable networks. Keep that option even when adding granular controls so that you benefit from those additional minor optimizations you enable.

You can specify them with the -T option and their number (0–5) or their name. The template names are paranoid (0), sneaky (1), polite (2), normal (3), aggressive (4), and crazy (5). The first two are for IDS evasion. Polite mode slows down the scan to use less bandwidth and fewer resources on the target machine. Normal mode is the default, and therefore -T3 does nothing. Aggressive mode speeds are increased assuming you are on a reasonably fast and reliable network. Finally, crazy mode assumes that you are on an extraordinarily fast network or that you are willing to sacrifice some precision for speed.


#### --Min / Max-hostgroup

--min-hostgroup <numhosts>; --max-hostgroup <numhosts>

Nmap has the ability to scan ports or scan versions from multiple hosts in parallel. Nmap does this by dividing the destination IP space into groups and then scanning one group at a time. In general, larger groups are more efficient. The downside is that the host results cannot be provided until the entire group has finished. So if Nmap started with a group size of 50, the user would not receive any reports (except for updates that are offered in verbose mode) until the first 50 hosts are completed.


#### --Min / Max-parallelism

--min-parallelism <numprobes>; --max-parallelism <numprobes>

These options control the total number of probes that can be pending for a group of hosts. They are used to scan ports and discover hosts. By default, Nmap calculates an ever-changing ideal parallelism based on network performance. If packets are dropped, Nmap slows down and allows fewer pending probes. The ideal probe number slowly increases as the network proves its worth. These options place minimum or maximum limits on that variable. By default, the ideal parallelism can be reduced to one if the network is unreliable and increased to several hundred in perfect condition.

### Service and version detection

Point Nmap to a remote machine and it might tell you that ports 25 / tcp, 80 / tcp, and 53 / udp are open. Using its nmap-services database of some 2,200 well-known services, Nmap would report that those ports likely correspond to a mail server (SMTP), web server (HTTP), and name server (DNS) respectively. This search is usually accurate: the vast majority of daemons listening on TCP port 25 are, in fact, mail servers. However, you shouldn't bet your safety on this! People can run services in strange ports.

```
-sV: services version detection
–All-ports do not exclude ports
–Version-all test each scan
–Version-trace trace version analysis activity
-Or activate detection of the Operating System
–Fuzzy guess OS detection
–Max-os-tries set maximum number of attempts against target system
```

#### -sV

Enable version detection, as discussed earlier. Alternatively, you can use -A, which enables version detection, among other things.

#### –O

Enable operating system detection, as discussed earlier. Alternatively, you can use -A to enable OS detection along with other things.

#### -Fuzzy

When Nmap cannot detect a perfect OS match, it sometimes offers similar possibilities. The match has to be very close for Nmap to do this by default. Either of these (equivalent) options makes Nmap guess more aggressively. Nmap will still tell you when an imperfect match is printed and will display your confidence level (percentage) for each guess.

### Bypass Firewalls / IDS

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

#### -F

-f fragment packets

The -f option causes the requested scan (including host discovery scans) to use small fragmented IP packets. The idea is to split the TCP header into multiple packets to make it harder for packet filters, intrusion detection systems, and other annoyances to detect what you're doing.

##### -D

-D d1, d2 cloak analysis with decoys

Conduct a decoy survey. This leads one to believe that the decoy computer / s are also polling the network. In this way their IDS can report that 5 to 10 port probes are being carried out from different IP addresses, but they will not know which IP address is performing the analysis and which are innocent decoys. Although this technique can be defeated by tracking the path of routers, response-dropping, and other active mechanisms, it is generally an effective technique to hide your IP address.

#### -S

-S ip spoof source address

Nmap may not be able to determine your IP address at times (Nmap will tell you if it happens). In this situation, you can use the -S option with the IP address of the interface through which you want to send the packets.

Another alternative use of this option is to falsify the address so that the analysis targets think that someone else is probing them.


### Other options

```
–Resume file continue analysis aborted (taking output formats with -oN or -oG)
-6 enable IPV6 scanning
-A aggressive, as with -O -sV -sC –traceroute
Interactive options
v / V increase / decrease analysis detail level
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

## Examples of useful commands

## Examples of useful commands

#### Read the list of hosts / networks from a file

`nmap -iL / path / test.txt`

#### Host / Network Exclusion

`nmap 192.168.1.0/24 - exclude 192.168.1.5,192.168.1.254`

#### To find out if a host / network is protected by a firewall

`nmap -sA scanme.nmap.org`

#### Scan a host if it is protected by the firewall

`nmap -PN scanme.nmap.org`

#### Scan a network and find out which servers and devices are running

`nmap -sP 192.168.1.0 / 24`

#### Show only open ports

`nmap -open scanme.nmap.org`

#### Shows all packages sent and received

`nmap -packet-trace scanme.nmap.org`

#### How can I scan specific ports?

```
 nmap -p [port] hostName
 # # Scan port 80
 nmap-p 80 192,168 0.1 0.1

 # # Scan TCP port 80
 nmap -p T: 80 192.168 0.1 0.1

 # # Scan UDP port 53
 nmap -p U: 53 192.168 0.1 0.1

 # # Scan two ports # #
 nmap -p 80, 443 192,168 0.1 0.1

 # # Scans port ranges # #
 nmap -p 80 -200 192.168 0.1 0.1

 # # Combine all options # #
 nmap -p U: 53, 111, 137, T: 21 -25, 80, 139, 8080 192.168 0.1 0.1
 nmap -p U: 53, 111, 137, T: 21 -25, 80, 139, 8080 server1.cyberciti.biz
 nmap -v -sU -sT -p U: 53, 111, 137, T: 21 -25, 80, 139, 8080 192.168 0.1 0.254

 # # Scan all ports with wildcard * # #
 nmap -p "*" 192.168 0.1 0.1

 # # Scan main ports, i.e. $ scan most common numeric ports # #
 nmap - TOP-ports 5 192.168 0.1 0.1
 nmap - top-ports 10 192,168 0.1 0.1
```

#### How can I detect the remote operating system?

```
 nmap -O 192.168 0.1 0.1
 nmap -O -osscan-Guess 192.168 0.1 0.1
 nmap -v -O -osscan-I guess 192.168 0.1 0.1
```

#### Scan a host using TCP ACK (PA) and TCP Syn (PS) ping

```
 nmap -PS 192.168.1.1
 nmap -PS 80,21,443 192.168.1.1
 nmap -PA 192.168.1.1
 nmap -PA 80,21,200-512 192.168.1.1
```

#### How can I detect remote services (server / daemon) version numbers?

```
nmap-sV 192.168.1.1
```

#### Scan a host for UDP services

`nmap -sU scanme.nmap.org`

#### Scan a firewall for security weakness

```
# # TCP Null Scan to trick a firewall into generating a response # #
 # # Set no bits (TCP header flag is 0) # #
 nmap -sN scanme.nmap.org

 # # TCP End scan to check firewall # #
 # # Sets only TCP FIN bit # #
 nmap -sF scanme.nmap.org

 # # TCP Xmas scan to check firewall # #
 # # Sets the END, PSH, URG and flags, lighting the pack like a Christmas tree # #
 nmap -sX scanme.nmap.org
```

#### Full TCP port scan using service version detection.

`nmap -p 1-65535 -sV -sS -T4 scanme.nmap.org`

More information on how the command works: https://explainshell.com/explain?cmd=nmap+-v+-sS+-A+-T4+target

## RECOMMENDED

```
nmap -v -sS -A -T4 target
nmap -v -sS -A -T5 target
nmap -v -sV -O -sS -T5 target
nmap -v -p 1-65535 -sV -O -sS -T4 target
nmap -v -p 1-65535 -sV -O -sS -T5 target
nmap -A -T4 -Pn --script=ssl-enum-ciphers,http-security-headers,http-waf-detect,vuln -------->  Ulises M. Alvarez @umalvarez
for((;;));do nmap -sP <randomIp/24> >> nameFile;done  -------->  @ov3rflow1 
nmap --min-rate 4500 --max-rtt-timeout 1500ms -p- {IP} --------> Funkadelic @0xSalle
nmap -sS -n -Pn -p- --max-rtt-timeout 100ms --min-parallelism 1000 {IP} --------> Funkadelic @0xSalle
nmap --max-retries=0 <ip>  -------->  Panico @Chungo_0
nmap -sV --script=vulscan/vulscan.nse www.example.com   -------->   r1ms3c @r1ms3c
nmap IP --open -oG resultados; cat resultados | grep "/open" | cut -d " " -f 2 > IPs_activas  -------->  alert(Y000!) @_Y000_
nmap -iR 10 -sn   -------->  alert(Y000!) @_Y000_
nmap -sV --script=banner <IP>   -------->   alert(Y000!) @_Y000_

```

## Nmap Scripting Engine

After seeing the basic options that this tool offers us, now we are going to focus on the scripts we have to improve our scans.

NSE has a complex implementation for efficiency, it is surprisingly easy to use. Just specify -sC to enable the most common scripts. Or specify the --script option to choose your own scripts to run by providing categories, script file names, or the name of directories filled with scripts that you want to run. You can customize some scripts by supplying them with arguments through the --script-args and --script-args-file options. --Script-help displays a description of what each selected script does. The remaining two options, --script-trace and --script-updatedb, are generally only used for debugging and script development. Script parsing is also included as part of the -A option (aggressive parsing)

Script scanning is normally performed in combination with a port scan, because the scripts may or may not run, depending on the port states found by the scan. With the -sn option it is possible to run a script scan without a port scan, only host discovery. In this case, only host scripts can be run. To run a script scan without a host discovery or port scan, use the -Pn -sn options in conjunction with -sC or --script. All hosts will be assumed to be up and still only host scripts will run. This technique is useful for scripts like whois-ip that only use the remote system address and do not require it to be active.

These are classified into the following categories:

```
Auth: run all your available scripts for authentication

Default: executes the basic scripts by default of the tool

Discovery: retrieves information from the target or victim

External: script to use external resources

Intrusive: uses scripts that are considered intrusive to the victim or target

Malware: check for open connections due to malicious code or backdoors

Safe: run scripts that are not intrusive

Vuln: discover the most known vulnerabilities

All: executes absolutely all available scripts with NSE extension
```

For this writing we are going to analyze the use of some of these scripts, in total there are 604 and you can find them here: https://nmap.org/nsedoc/

#### Most popular scripts

When it comes to more than 600 scripts, it is not easy to find the most popular ones by inspecting them one by one. This is why the Nmap team has created a "-sC" option, which allows you to run the main Nmap scripts at the same time.

`nmap -sC scanme.nmap.org`

#### Scripts within a category.

Other times, you will need to run all the scripts within a category. This can be done using the name of the "script category", as you can see here:

examples:

```
nmap --script discovery scanme.nmap.org
nmap --script default, safe scanme.nmap.org
```

#### Scripts with a wildcard

Nmap also allows you to run scripts using wildcards, which means that you can target multiple scripts that end or end with any pattern. For example:

```
nmap --script "ftp - \ *" scanme.nmap.org
nmap --script "ssh - \ *" scanme.nmap.org
```

#### Run a single Nmap script

This is the perfect solution when you already know what script will be used. For example, if we want to run the http-brute script to perform a brute force password audit against http basic, digest and ntlm authentication, we will use:

`nmap --script =" http-brute "192.168.122.1`

### Useful scripts

#### http-enum

The script allows us to brute-force a server path to discover web applications in use. Test more than 2000 server paths.

https://twitter.com/_Y000_/status/1206452331967139842

#### ssl-poodle

we are going to detect if a page is vulnerable to "sslv3 supported (poodle attack others)" (cve-2014-3566)

https://twitter.com/_Y000_/status/1206336129835896833

#### http-grep

The http-grep script looks for useful information on the given page. By default, it returns the email addresses and IP addresses found on all subpages discovered by the script.

#### ssh-brute

The ssh-brute script is used to break passwords for SSH services with predictive text input. By default, it uses its own database, rather a large database of users and passwords.

#### dns-brute

The dns-brute script tries to find as many subdomains as the host is being tested using the most frequently used subdomain names.

#### http-config-backup

The http-config-backup script sends a lot of queries to the web server, trying to get a copy of the popular CMS configuration left by the user or the text editor.

#### smb-enum-users

The task of the smb-enum-users script is to list all available users on the scanned Windows system.

#### firewalk

The Firewalk script, using the packet tracing technique (traceroute) and TTL passing, known as firewalking, attempts to detect firewall / gateway rules.

#### mysql-empty-password

The mysql-empty-password script checks whether it is possible to log into the MySQL server to the root or anonymous account using an empty password.

#### mysql-users

The mysql-users script is used to list the users available on the MySQL server. To list, we will use the root account with an empty password, which we discovered in the previous script.

#### mysql-brute

The mysql-brute script is used to break access data to the MySQL server. The above script has detected the user tester whose password we will now try to break.

#### http-sitemap-generator

This script allows us to make a sitemap of the pages, generally used more on ports 80 and 443

https://twitter.com/_Y000_/status/1206614332538314752

#### --traceroute + --script traceroute-geolocation

script allows us to list the geographic locations of each jump in a tracking route and optionally saves the results in a KML file, which can be plotted on Google Earth and on maps.

https://twitter.com/_Y000_/status/1206678017856151553

## XSS and SQLi?

### XSS

Nmap scripts that will help us find xss errors:

```
http-stored-xss
http-phpself-xss
http-xssed
http-unsafe-output-escaping
```

https://twitter.com/_Y000_/status/1261309010331877376


### SQLi

Nmap scripts that will help us find SQLi errors:

```
http-sql-injection
http-unsafe-output-escaping
```

https://twitter.com/_Y000_/status/1266626563384066048


### Scripts for WORDPRESS

```
http-wordpress-users
http-wordpress-enum
http-wordpress-brute
```

## VULSCAN

Nmap also has the help of third parties who contribute their scripts to the community, and this is appreciated too much since we have to talk about this script.

What this script does is search for vulnerabilities directly from a database and shows us the information in case it finds something.

https://github.com/scipag/vulscan

Vulscan is a module that upgrades nmap to a vulnerability scanner. The nmap -sV option enables per-service version detection that is used to determine possible failures based on the identified product. The data is fetched in an offline version of VulDB.
script = vulscan / vulscan.nse

### Installation

Install the files in the following folder of your Nmap installation:

`Nmap\scripts\vulscan\*`

Clone the GitHub repository like this:

```
git clone https://github.com/scipag/vulscan scipag_vulscan
ln -s `pwd` / scipag_vulscan / usr / share / nmap / scripts / vulscan
```

## AUTOMATING

Lastly, I would like to encourage you to automate your scans. As an example I will talk about 3 tools that I created for my personal use but they are published in my github account.

### NSE-NMAP

#### What is nse-nmap?

It is a tool written in bash that was created specifically to automate the scans used in nmap.

We will find a menu where the scans are organized into 3 types:

```
-Basics
-Advanced
-NSE
```

In the Basics category we will find scans:

```
-verify living goals
-live objectives in a range of ip's
- SYN, FIN, UDP scans
```

In the Advanced category we find scans:

```
- Protocol scan
-Scanning of services
-Fingerprint scan (root)
-Aggressive scan
-Traceroute scan (root)
-Whois scan
```

finally in NSE we find the NSE scripts:

```
-Http enumeration
-Http title
-Gross dns strength
-Find HTTP errors
-EXIF data from photos
-Brude force to FTP
-Brude force to mysql
-Firewall detection
-Fingerprint of Firewall
```

You can find it here: https://github.com/Y000o/nse-nmap

### Vulmap

Vulmap is a basic tool to search for vulnerabilities in websites, with this tool we can search for vulnerabilities in sites created in Wordpress, xss injections and SQL injections.

You can find it here: https://github.com/Y000o/vulmap

### Brute-map

Brute-map is an automated brute force scans to different services.

You can find it here: https://github.com/Y000o/Brute-map

## Thanks

Before finishing this writing I would like to thank all the people who take the time to read my writings, I know that I am not the best exponent in this, I have as many mistakes as anyone else, for that I thank you ... forever be there supporting either providing content or helping me improve my writing once they are finished.

As I always comment after each writing, please feel free to tell me any questions, any errors, something that you would like to improve or contribute.

Thank you very much!.
