---
title: Nmap
date: 2022-12-04 16:35:00 +00:00
categories: [tools]
tags: [tools]     # TAG names should always be lowercase
---

## What is Nmap?

Nmap (Network Mapper) is a free and open-source network scanner created by Gordon Lyon. Nmap is used to discover hosts and services on a computer network by sending packets and analyzing the responses.

## Few Features of Nmap
*   Host discovery – Identifying hosts on a network. For example, listing the hosts that respond to TCP and/or ICMP requests or have a particular port open.
*   Port scanning  – Enumerating the open ports on target hosts.
*   Version detection – Interrogating network services on remote devices to determine application name and version number.
*   OS detection – Determining the operating system and hardware characteristics of network devices.
*   Scriptable interaction with the target – using Nmap Scripting Engine (NSE) and Lua programming language.

# Learning nmap features with examples
## Target Scanning
### Scanning a single target
```console
nmap 127.0.0.1
```

### Scanning a host
```console
nmap mydomain.com
```

### Scanning multiple targets
```console
nmap 127.0.0.1 192.168.0.1
```

### Scanning a whole network
```console
nmap 127.0.0.1/24
```

### Scanning multiple targets using IP range
```console
nmap 127.0.0.1-210
```

### Scanning targets from a file
```console
nmap -iL targets.txt
```
Content of targets.txt
```
127.0.0.1
192.168.1.1
192.168.1.255
```

### Excluding a target from a IP range
```console
nmap 192.168.1.1/24 --exclude 192.168.1.1
```

## Scanning Techniques
### TCP SYN Scan -sS
```console
sudo nmap -sS 192.168.1.2
```
Here the Nmap first sends the TCP SYN packet to the port it is scanning and if the port is open, it acknowledges by sending a SYN ACK. Then to complete a full TCP handshake our Nmap is supposed to send a ACK packet which it does not and closes the connection. And the full TCP connection is never established. As the connection was never established the server won't have the log of us scanning the network and also we now know if the port was open or not.

### TCP connect Scan -sT
```console
nmap -sT 192.168.1.2
```
This is the default scan done by the nmap. It doesnot requires sudo priviliges like the TCP syn scan and it establishes the connection using the 3 way TCP handshake.

### UDP Scan (-sU)
```console
sudo nmap -sU 192.168.1.2
```
Like the name suggests, it is used for scanning open UDP ports on the network. UDP port scanning is usually very slower and requires sudo privileges.

### Ping Scan (-sP)
```console
sudo nmap -sP 192.168.1.1/24
```
Ping scan is used just to check whether the device is on or not. It also requires root privileges, otherwise it just uses the usual TCP connect scan.

## Host Discovery
### List all targets
```console
nmap -sL 192.168.1.1/24
```
It does not scan the targets but only lists them.

### Host discovery only ( No Port Scanning)
```console
nmap -sn 192.168.1.1/24
```

### Port Scanning only ( No Host discovery)
```console
nmap -Pn 192.168.1.3
```
As some targets can have rules not to reply to ping requests to avoid denial of service attacks, \-Pn does not check if the host is up or not and continues to do the port scan. 

### Avoiding DNS resolution (-n)
```console
nmap -n 192.168.1.1/24
```

## Specifying Ports on our scan
### Top 1000 ports
```console
nmap 127.0.0.1
```
The default scan checks for top 1000 ports. Top 1000 ports does not mean ports in  range 1 - 1000 but the according the frequency of occurence of ports. For example it is highly likely for a webserver to have port 443 and port 80 open.

### Scanning a range of Ports
```console
nmap -p 1-1000 127.0.0.1
```
### Scanning all 65535 ports
```console
nmap -p- 127.0.0.1
```
### Scanning a single port
```console
nmap -p 22 127.0.0.1
```
### Fast scan
```console
nmap 127.0.0.1 -F
```
It is a fast scan and only scans the top 100 ports.

### Scanning top N ports
```console
nmap --top-ports 2000 127.0.0.1
```
This scan for top 2000 ports.

# Output format
### Output in normal format
```console
nmap -oN scan.log 127.0.0.1
```
This saves the result of the scan in filename scan.log in normal format.

### Output in  xml file
```console
nmap -oX scan.xml 127.0.0.1
```
### Output in greppable format
```console
nmap -oG scan.log 127.0.0.1
```
### Output in all format
```console
nmap -oA scan 127.0.0.1
```

### Verbosity
```console
nmap 127.0.0.1 -v
```
We can get extra information by using -v (verbose) flag. -vv flag increases the level of verbosity.

### Debugging
```console
nmap 127.0.0.1 -d
```
It increase the debugging level. We can use -dd flag for greater debugging effect.

# Speed of scanning
### Paranoid
```console
nmap 127.0.0.1 -T0
```
### Sneaky
```console
nmap 127.0.0.1 -T1
```
Paranoid and Sneaky method are used for IDS evasion.
### Polite
```console
nmap 127.0.0.1 -T2
```
Polite mode slows down the scan to use less bandwidth and target machine resources. 

### Normal
```console
nmap 127.0.0.1 -T3
```
This is default scanning mode.

### Aggressive
```console
nmap 127.0.0.1 -T4
```
Aggressive mode speeds scans up by making the assumption that you are on a reasonably fast and reliable network.
### Insane
```console
nmap 127.0.0.1 -T5
```
Insane mode assumes that you are on an extraordinarily fast network or are willing to sacrifice some accuracy for speed

The faster you want to complete your scan, increase the number from 1 to 5. But with higher value in flag, the number of requests will be very high which might trigger the firewall or IDS if they are in place.

### Specifying rate for scanning
```console
nmap -p- --min-rate 10000 127.0.0.1
```
Scanning for all the open ports on a target takes time. So we can tell nmap to send packets no slower than **N** number per second, in this case it is 10000 packets per second.
And also we can specifiy the maximum rate of sending packets per second.

```console
nmap --max-rate 10 127.0.0.1
```

### Specifying maximum number of retries
```console
nmap -p- 127.0.0.1 --max-retries 0
```
This flag specifies the number of times a packet is to be resent on a port to check if it is open or closed. Setting its value to 0 can speed up the process but decreases the accuracy.

## Version Discovery
```console
nmap -sV 127.0.0.1
```
Nmap tries to detemine the version of the sevice running on the port which if correctly determined can be very useful later on during a penetration testing.

## OS Discovery
```console
nmap -O 127.0.0.1
```
Nmap tries to detemine the Operating System that our target is running using TCP/IP stack fingerprinting.

## Nmap Scripting Engine (NSE)
This is one of the most and powerful features of nmap. It allows users to write (and share) simple scripts (using the Lua programming language ) to automate a wide variety of networking tasks. Those scripts are executed in parallel with the speed and efficiency you expect from Nmap. 

Currently defined categories are **auth, broadcast, default. discovery, dos, exploit, external, fuzzer, intrusive, malware, safe, version, and vuln**. As the name suggests, **version** scripts help to determine the version of currently running service, the **vuln** scripts are used to check if the currently running service is vulnerable and so on.

Theses scripts can be found inside folder **/usr/share/nmap/scripts**.
```console
/usr/share/nmap/scripts$ ls | wc -l
599
```
The version of nmap that I am currenly running has a total of 599 scripts.

### Scanning with default scripts
```console
nmap -sC 127.0.0.1
```
It performs the scan with default NSE scripts.

### Specifying the type of scripts
```console
nmap --script=vuln 127.0.0.1
```
It scans the targets which the scripts which are marked as **vuln**.
#### Listing the vuln scripts
```console
$/usr/share/nmap/scripts$ ls | grep -i vuln
http-vuln-cve2006-3392.nse  
http-vuln-cve2009-3960.nse
http-vuln-cve2010-0738.nse
http-vuln-cve2010-2861.nse
.....
.....
smb-vuln-ms10-061.nse      
smb-vuln-ms17-010.nse      
smb-vuln-regsvc-dos.nse   
smb-vuln-webexec.nse      
smtp-vuln-cve2010-4344.nse
smtp-vuln-cve2011-1720.nse  
smtp-vuln-cve2011-1764.nse                                               
```
### Running scripts with wildcard
```console
nmap --script="http*" 127.0.0.1
```
It scans the target with scripts starting with **http**.

## All in one
```console
nmap -A 127.0.0.1
```
Enable OS detection, version detection, default script scanning, and traceroute.



# Combining what we have learned so far

```console
$:/usr/share/nmap/scripts$ nmap -sC -sV -oN nmap-scan 192.168.1.2
```
We scan the target with ip 192.168.1.2 for top 1000 open ports along with version detection of the service running, uses default scripts and the ouput will be saved on the normal nmap-scan file.

```console
$:/usr/share/nmap/scripts$ nmap -p- -A -T4 -oA nmap/nmap-scan 192.168.1.2
```
Here we are scanning target with ip address 192.168.1.2 for all open ports and -A flag which will enable OS detection, version detection, traceroute as well as uses default scripts and save the output inside a nmap directory with file name nmap-scan.

Nmap also offers a lot of other functionalities. If you have any problems, I suggest you to check the man page for nmap using `man nmap`.


## References
[https://www.stationx.net/nmap-cheat-sheet/](https://www.stationx.net/nmap-cheat-sheet/)   
[https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/](https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/)   
[https://en.wikipedia.org/wiki/Nmap](https://en.wikipedia.org/wiki/Nmap)

