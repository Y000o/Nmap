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
     * [Bypassing Firewalls / IDS](#Bypassing-Firewalls / IDS)
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


