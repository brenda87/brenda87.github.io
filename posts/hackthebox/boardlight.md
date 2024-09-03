---
layout: default
title : brenda - headless HTB Writeup
---

![Screenshot (506)](https://github.com/user-attachments/assets/de43d77c-fb39-4630-a345-2cb5d70efdbc)

`Boardlight` is an easy machine in HTB.

### Recon
### Nmap scan

Deployed an Nmap scan on the target IP, uncovering the open ports and exposed services. 

![Screenshot (494)](https://github.com/user-attachments/assets/89a0faf8-7d44-4591-b2f9-4d56ec38584e)

After running an Nmap scan, I found two open ports: SSH and HTTP. The HTTP service was running Apache version 2.4.41, which had two low-risk vulnerabilities (CVE-2020-1927 and CVE-2020-1934). I didn’t dive too deep into them since they were low risk.

Added the target IP to /etc/hosts to access the website directly. 

![Screenshot (495)](https://github.com/user-attachments/assets/e3d7ec7a-6c91-4743-a131-46c380f2a033)

### Directory search

I used `dirsearch` to search for directories but I didn't find any userful

![Screenshot (496)](https://github.com/user-attachments/assets/5e2f3cca-3f66-41bc-ad8e-f3d06d5090fc)

I employed `ffuf` to scan for subdomains on `board.htb`, revealing the existence of `crm.board.htb`. 

![Screenshot (497)](https://github.com/user-attachments/assets/9c0bb27b-8303-4a1b-8f09-23ef3d4dbf91)

After adding crm.board.htb to /etc/hosts, I navigated to the website for further exploration.

![Screenshot (498)](https://github.com/user-attachments/assets/3737f128-10ce-498e-b287-e6b32e2cba6e)

I checked out the website and it opened up a Dolibarr page running version 17.0.0.

![Screenshot (507)](https://github.com/user-attachments/assets/0a2e44d4-d0b7-4565-be28-a066b9fd734c)

Searched for the default Dolibarr login and found that both the username and password are 'admin,' and it worked.

![Screenshot (509)](https://github.com/user-attachments/assets/1f78da5f-7735-408f-9355-ba7802e312f8)

Then I searched for vulnerabilities in Dolibarr version 17.0.0 and came across CVE-2023-30253, which has a reverse shell PoC exploit.

![Screenshot (510)](https://github.com/user-attachments/assets/c7867b30-e9c0-4a58-8c31-f8ba43bbc78d)

I exploited the vulnerability and got a reverse shell.































