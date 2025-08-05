# Matrix-Breakout: 2 Morpheus Writeup

![banner image](screenshots/banner.png)

### Description:This is the second in the Matrix-Breakout series, subtitled Morpheus:1. It’s themed as a throwback to the first Matrix movie. You play Trinity, trying to investigate a computer on the Nebuchadnezzar that Cypher has locked everyone else out from, which holds the key to a mystery.

### [download machine](https://vulnhub.com/entry/matrix-breakout-2-morpheus,757/)

### Phase 1: Reconnaissance
Getting the IP Address of the Target machine I used Netdiscover tool and got the ip address → 192.168.161.131

![discover image](screenshots/discover.png)

### Phase 2: Network Scanning

```bash
nmap -sV 192.168.161.131
```

![nmap scan](screenshots/nmap.png)

Here port 80 http is open, lets open it in browser and see what it shows

![webpage](screenshots/webpage.png)

Following is the webpage that is present on target machine, and nothing interesting in source and nothing in cookies and localstorge

Now I will use Gobuster to get the endpoints/dirs for this webapp that are hidden

```bash
gobuster dir -u http://192.168.161.131/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x .php,.txt,.html
```

![gobuster enum](screenshots/gobuster.png)

So I got the following results:
/robots.txt
/graffiti.txt
/graffiti.php

Lets visit robots.txt,
Got this LOL -> There's no white rabbit here. Keep searching!

-> Let try graffiti.txt
It is just a text file nothing special

Lets try graffiti.php here we got a web page with a input box and also we can do xss attact from this

![homepage](screenshots/homepage.png)