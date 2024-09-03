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

