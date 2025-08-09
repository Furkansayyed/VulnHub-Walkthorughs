# 🔐 Hackable III Write Up

### **Description** Focus on general concepts about CTF
Difficulty: Medium

### Phase 1: Reconnaissance
Gettting the ip address of the target machine using arp-scan

![ip address](screenshots/ip.png)

> Target ip -> 192.168.100.7

```bash
                                                                                                               
┌──(kali㉿kali)-[~]
└─$ nmap -sV 192.168.100.7
Starting Nmap 7.95 ( https://nmap.org ) at 2025-08-09 04:16 EDT
Nmap scan report for 192.168.100.7
Host is up (0.0021s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE    SERVICE VERSION
22/tcp filtered ssh
80/tcp open     http    Apache httpd 2.4.46 ((Ubuntu))
MAC Address: 08:00:27:07:12:B6 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.99 seconds

```
Here only port 22 and 80 are open, lets try to use port 80 http service in browser

