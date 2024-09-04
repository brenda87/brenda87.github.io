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

After running an Nmap scan, I found two open ports: SSH(22) and HTTP(80). 

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

![Screenshot (532)](https://github.com/user-attachments/assets/4983cea9-b5e6-468f-bdbe-f3aa2ad9eba9)

![Screenshot (525)](https://github.com/user-attachments/assets/4e77dd65-5468-4f22-a066-8780ca493553)

In the directory www-data@boardlight:~/html/crm.board.htb/htdocs$, we notice several directories, including a "conf" directory. Navigating to it using cd conf, we discover three configuration files. Upon inspecting them, we find one particularly interesting file, conf.php, containing valuable information. Contents of conf.php reveals Username & Password

![Screenshot (527)](https://github.com/user-attachments/assets/eabdd1d0-4537-41b2-888b-f015ea62f109)

Checked /etc/passwd file’s content, found 2 shell users. One is root and the other one is larissa. Logged in as user larissa with using the password we found before by using ssh.
We successfully logged in via SSH as the larissa user, allowing us to retrieve the user flag!

![Screenshot (528)](https://github.com/user-attachments/assets/27d99322-9042-4359-b6da-dba717a161c9)

## Privilege Escalation

I examined the SUID files and identified that one of them was running Enlightenment version 0.23.1, which is known to be vulnerable. This SUID file was exploited using CVE-2022-37706.

![Screenshot (530)](https://github.com/user-attachments/assets/95c94f00-3fd0-4d18-9b29-4363d361029e)

And there I got root flag!!

![Screenshot (531)](https://github.com/user-attachments/assets/eb2af408-d858-4cbb-8b67-9b6d3fddddb4)


* * *


<footer>
    <p>&copy; 2024 Brenda. All rights reserved.</p>
  </footer>






























