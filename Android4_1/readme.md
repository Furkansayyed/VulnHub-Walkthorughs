# 📳📳 Android4: 1 Writeup 📳📳

## Getting the Ip Address of Target Device:

![device screen](screenshots/screen.png)

I will be using arp-scan to scan my local network and get the ip address of the target device

> target ip -> 192.168.100.11

## Scanning the Target 

Using nmap to scan the device for open ports

```bash

┌──(kali㉿kali)-[~/Desktop/Machines_WriteUps/Android_1]
└─$ nmap -sV 192.168.100.11 -oN nmap_full_scan
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-06 05:40 EDT
Nmap scan report for 192.168.100.11
Host is up (0.0031s latency).
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE  VERSION
5555/tcp open  freeciv?
8080/tcp open  http     PHP cli server 5.5 or later
MAC Address: 08:00:27:ED:55:9B (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.30 seconds

```

## Exploitation

Let's go with port 8080, it is a http service, lets open it up in browser and see if we got some clues

![http service](screenshots/browser.png)

Here I dont get any clues or flag in the http services I even used Gobuster to find the hidden dirs but nothing interesting...