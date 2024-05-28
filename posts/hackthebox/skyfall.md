---
layout: default
title : brenda - skyfall HTB Writeup
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

I employed `ffuf` to scan for subdomains on `skyfall.htb`, revealing the existence of `demo.skyfall.htb`. 

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(74).png)

After adding demo.skyfall.htb to /etc/hosts, I navigated to the website for further exploration.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(76).png)

Using the demo account details, I successfully logged into the page and gained access to its contents.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(77).png)

The system identified itself as a MinIO storage system. However, I managed to bypass it by appending `%0A` to the URL.
Towards the bottom of the page, I stumbled upon a promising domain that piqued my interest.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(78).png)

Added domain to /etc/hosts

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(80).png)

### USER FLAG

While researching MinIO vulnerabilities, I stumbled upon a GitHub article highlighting CVE-2023-28432, an information leak vulnerability. Exploiting this flaw could potentially expose sensitive data.
[Article](https://github.com/gobysec/CVE-2023-28432)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(79).png)

Using Burp Suite to intercept traffic from `http://prd23-s3-backend.skyfall.htb/minio/bootstrap/v1/verify`, I uncovered sensitive information: 
`MINIO_ROOT_USER: 5GrE1B2YGGyZzNHZaIww` 
`MINIO_ROOT_PASSWORD: GkpjkmiVmpFuL2d3oRx0`.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(81).png)

Next, I used the provided [link](https://github.com/minio/mc) to install mc (MinIO Client) to manage the MinIO container.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(84).png)

I then logged into the MinIO container to view all the files.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(95).png)

I retrieved the `home_backup.tar.gz` file from the MinIO container.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(86).png)

I searched through the retrieved files for a Vault token.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(97).png)

I exported the Vault address and token, then logged in using the Vault token. Using Vault, I queried for a list of SSH roles.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(91).png)

I logged in via the dev_otp_key_role and utilized the provided OTP as the password to gain access.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(98).png)

With a triumphant "boom," I successfully retrieved the `user flag`, marking a significant milestone in my journey through Skyfall.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/skyfall/Screenshot%20(90).png)















