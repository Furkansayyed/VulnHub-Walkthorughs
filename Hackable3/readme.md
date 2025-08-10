# 🔐 Hackable III Write Up

### **Description** Focus on general concepts about CTF
Difficulty: Medium

### Phase 1: Reconnaissance
Gettting the ip address of the target machine using arp-scan

![ip address](screenshots/ip.png)

> Target ip -> 192.168.100.7

### Phase 2: Network Scanning and Enumeration

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
Here ssh port is in filtered state and only 80 is open, lets try to use port 80 http service in browser

I pasted the target ip in browser, but on webpage nothing interesting, now i will be usign Go Buster to get the hidden endpoints using gobuster dir method and a kali wordlist

```bash
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u 192.168.100.7 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.100.7
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/css                  (Status: 301) [Size: 312] [--> http://192.168.100.7/css/]
/js                   (Status: 301) [Size: 311] [--> http://192.168.100.7/js/]
/config               (Status: 301) [Size: 315] [--> http://192.168.100.7/config/]
/backup               (Status: 301) [Size: 315] [--> http://192.168.100.7/backup/]
/imagens              (Status: 301) [Size: 316] [--> http://192.168.100.7/imagens/]
/login_page           (Status: 301) [Size: 319] [--> http://192.168.100.7/login_page/]
/server-status        (Status: 403) [Size: 278]
Progress: 220560 / 220561 (100.00%)
===============================================================
Finished
===============================================================

```
Lets Enumurate each endpoint we got in result
