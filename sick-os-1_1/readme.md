# Sick OS write Up 

I got the ip address for the target machine using:
```bash

sudo arp-scan -l

```

Target ip -> 192.168.100.9

### Network Scanning 

```bash

# Nmap 7.95 scan initiated Wed Aug 20 03:03:28 2025 as: /usr/lib/nmap/nmap --privileged -sV -o nmap_full_scan 192.168.100.9
Nmap scan report for 192.168.100.9
Host is up (0.0030s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE  SERVICE    VERSION
22/tcp   open   ssh        OpenSSH 5.9p1 Debian 5ubuntu1.1 (Ubuntu Linux; protocol 2.0)
3128/tcp open   http-proxy Squid http proxy 3.1.19
8080/tcp closed http-proxy
MAC Address: 08:00:27:1C:91:68 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Aug 20 03:03:45 2025 -- 1 IP address (1 host up) scanned in 16.43 seconds

```

Open Ports are 22(ssh) and 3128(htttp-proxy) 
Here 22 has no vulnearability 

3128 has Squid http proxy, to access the proxy I used foxy proxy, and to navigated to 127.0.0.1
![homepage of target server](screenshots/homepage.png)

I navigated to robots.txt and got the path to /wolfcms which redirected to the wolf cms homepage
![wolf cms homepage](screenshots/wolf_1.png)

