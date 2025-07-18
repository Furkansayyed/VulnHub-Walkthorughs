# Setting Up the Virtual Lab for Machine, I am using Virtual Box 
# Creating a Virtual Lab
1. create a NAT Network in Virtual Box
![alt text](image.png)

2. Go to Network Setting and connect the machines network to created NAT Network
![alt text](image-1.png)

3. Now Start Both the Machines

# Phase 1: Reconnaissance

This VM gets it’s IP address from the DHCP, so through a quick network scan, I saw that the machine was running on 192.168.100.4

For a convenince, I have given a host to server 192.168.100.4 as hacknest.net
![alt text](screenshots/image-3.png)


 Time to make some notes again. There are two main services active across three ports. An SSH server and an Apache webserver. We can also see it’s running on a VirtualBox, not that it matters for our assessment or not like we didn’t already know this.

# Phase 2: Information Gathering
As port 80 is open let's open it up in browser,
![alt text](screenshots/webpage.png)

On the website, a boot animation loads, giving us some sort of fake web shell. Upon further investigation, I noticed that the commands don’t really do anything special, other than giving some videos

As it is a web server lets open robots.txt,

![alt text](screenshots/robot.png)

### BAMM !! 🔓 We got our our first flag 
Download the Key 1 file to get the flag

![alt text](screenshots/flag_1.png)