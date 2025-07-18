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

We also got a fsocity.dic, I come to know from its extension that it is a dictionary file, now lets check its content,I sorted and filtered unique values from file and got around 11k words

![](screenshots/sort.png)

### Let's Scan the website for login page to get adminstrative rights for website

Here I am using nikto to scan the web server

`nikto -h hacknest.net `

#### Here's the result of the Nikto and we got that website is running on wordpress and also the login URL..

![nikto output](screenshots/login_scan.png)


![wordpress Login Page](screenshots/login_page.png)


# Phase 3: Exploitation
The great thing about this WordPress version is that you can enumerate usernames of users on the WordPress installation. How? Basically as follows:

If you write some random gibberish in the login form, you will get the following error:

> ERROR: Invalid username. Lost your password?

If, however, you get a valid username, you’ll get a different error:

> ERROR: The password you entered for the username pablo is incorrect. Lost your password?

Knowing this, we can try a bunch of usernames and check if there is any difference in the error messages. We have our wordlist from before, so it would be my first guess to try that one.

The tool we’re going to be using is hydra. The best tool for bruteforcing anything. But before we go ahead and launch it, we need to check what the requests to log into a WordPress site looks like.

In order to capture that request, I’m going to use Burp Suite as a proxy in between my browser and the target.

![burp request](screenshots/burp.png)

We see 3 important parameters:

-   **log**  — The username
-   **pwd**  — The password
-   **wp-submit**  — The “thing” we’re submitting (log in)


Let’s throw this into hydra:

> hydra -L fsocity_filtered.dic -p something 192.168.1.56 http-post-form ‘/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username’


-   -L fsocity_filtered.dic — Here we will bruteforce the login using our wordlist we found earlier
-   -p — Here we supply a password, at this stage, we only want the username, so it doesn’t matter what we fill in here
-   192.168.1.56 — Our target
-   http-post-form — The method we’re bruteforcing, in this case, a POST request

![hydra_username](screenshots/username.png)