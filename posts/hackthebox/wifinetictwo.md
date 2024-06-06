---
layout: default
title : brenda - headless HTB Writeup
---


![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(180).png)

`WifineticTwo` is a medium machine in HTB.

### Recon
### Nmap scan

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20().png)

Added target ip to /etc/hosts using these commands:

`echo"10.10.11.7 wifinetictwo.htb" | sudo tee -a /etc/hosts`

I then opened the website `wifinetictwo.hb:8080/`. And got this!

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(151).png)

I searched online for the default credentials for OpenPLC and used them to log in.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(162).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(154).png)

The OpenPLC webserver runs PLC programs from the OpenPLC editor and allows configuration of runtime parameters. Currently, the dashboard shows the loaded file is blank_program.st and the server is stopped.

### CVE-2021-31360

If you search for OpenPLC vulnerabilities, you'll find [CVE-2021-31630](https://www.cvedetails.com/cve/CVE-2021-31630) and this proof of concept: [exploit-db.com/exploits/49803](https://www.exploit-db.com/exploits/49803).

































