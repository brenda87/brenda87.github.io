---
layout: default
title : brenda - bizness HTB Writeup
---



![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(68).png)

Bizness" is your trusty training ground—an easy box designed to warm up your hacking muscles. 

### Nmap Scan
Kicking off the mission, we deployed an Nmap scan on the target IP, uncovering the open ports and exposed services. 

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(27).png)

The Nmap scan was unproductive, so I added the target IP to /etc/hosts to access the website directly. 

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(69).png)

### Directory search

I conducted a directory search of the `bizness.htb` website using `dirsearch`, uncovering hidden directories and potentially vulnerable endpoints for further investigation.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(30).png)

I navigated to /bizness.htb/accounting/ and encountered a login page, indicating potential access to sensitive financial information or administrative controls.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(28).png)

### Vulnerability research

I researched OFBiz vulnerabilities and stumbled upon a relevant article, suggesting potential exploits that could be leveraged within the OFBiz framework.

[Reference: Apache-OFBiz-Authentication-Bypass](https://github.com/jakabakos/Apache-OFBiz-Authentication-Bypass)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(29).png)

### Exploitation

 I cloned the GitHub repository containing the essential files for exploiting the identified OFBiz vulnerabilities

 ![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(31).png)

Launching the Python exploit, I set up Netcat eagerly. Boom! A wild shell of user "ofbiz" appeared, opening the door to system secrets!

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(32).png)

As I delved deeper, a gleaming treasure awaited: the coveted user flag!

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/bizness/Screenshot%20(34).png)

### Privilege Escalation


















