---
layout: default
title : brenda - bizness HTB Writeup
---



![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(68).png)

`Bizness` is your trusty training ground—an easy box designed to warm up your hacking muscles. 


### Nmap Scan
Kicking off the mission, we deployed an Nmap scan on the target IP, uncovering the open ports and exposed services. 

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(27).png)

The Nmap scan was unproductive, so I added the target IP to /etc/hosts to access the website directly. 

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(69).png)


### Directory search

I conducted a directory search of the `bizness.htb` website using `dirsearch`, uncovering hidden directories and potentially vulnerable endpoints for further investigation.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(30).png)

I navigated to `/bizness.htb/accounting/` and encountered a login page, indicating potential access to sensitive financial information.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(28).png)


### Vulnerability research

I researched OFBiz vulnerabilities and stumbled upon a relevant article, suggesting potential exploits that could be leveraged within the OFBiz framework.

[Article: Apache-OFBiz-Authentication-Bypass](https://github.com/jakabakos/Apache-OFBiz-Authentication-Bypass)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(29).png)


### Exploitation

 I cloned the GitHub repository containing the essential files for exploiting the identified OFBiz vulnerabilities

 ![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(31).png)

Launching the Python exploit, I set up Netcat eagerly. Boom! A wild shell of user "ofbiz" appeared, opening the door to system secrets!

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(32).png)

As I delved deeper, a gleaming treasure awaited: the coveted user flag!

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(34).png)


 I upgraded my shell with this incantation: `script /dev/null -qc /bin/bash
stty raw -echo; fg; ls; export SHELL=/bin/bash; export TERM=screen; stty rows 38 columns 116; reset;`

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(35).png)


### Privilege Escalation

In my digital expedition, I stumbled upon a hidden gem: the intriguing file `AdminUserLoginData.xml`, tucked away in the depths of `/opt/ofbiz/framework/resources/templates`. 

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(38).png)

In a twist of fate, the discovered password from the templates proved futile in our quest.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(39).png)

I chanced upon a curious file named `c54d0.dat` hiding in the shadows of `/opt/ofbiz/runtime/data/derby/ofbiz/seg0`. Opening it, I discovered a secret code—a hash guarding the current password! 

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(39).png)

 I stumbled upon an article detailing an Apache-OFBiz-SHA1-Cracker. This tool promised to crack the SHA1 hash protecting the password I found
 
[Article: Apache-OFBiz-SHA1-Cracker](https://github.com/duck-sec/Apache-OFBiz-SHA1-Cracker)

 ![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(41).png)

  I executed 'su' and entered the newfound password, and "BOOM!" I claimed the ultimate prize: the root flag!

   ![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(42).png)


   Until next time, farewell! Onward to new adventures!



* * *


<footer>
    <p>&copy; 2024 Brenda's Cyber Fortress. All rights reserved.</p>
  </footer>


















