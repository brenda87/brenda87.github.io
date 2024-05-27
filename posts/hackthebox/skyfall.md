---
layout: default
title : brenda - bizness HTB Writeup
---

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(94).png)

`Skyfall` is an Insane machine of HTB, tough, challenging, and definitely not for the faint-hearted!


### Recon

### Nmap Scan
`Nmap` detects two open TCP ports on Skyfall: SSH (22) and HTTP (80). They weren't of much help.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(70).png)

I then added the target IP to `/etc/hosts` directory to access website.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(71).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(72).png)


### Directory search

I used `dirsearch` to search for directories but I didn't find any userful

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(73).png)

Using `ffuf` I checked for any subdomains in `skyfall.htb`. 

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(74).png)




