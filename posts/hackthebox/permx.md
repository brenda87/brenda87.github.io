---
layout: default
title : brenda - skyfall HTB Writeup
---

![Screenshot (511)](https://github.com/user-attachments/assets/88c07fa7-5e39-452a-9567-2b759221a63b)

`PermX` is an easy machine in HTB.

### Recon
### Nmap scan

Deployed an Nmap scan on the target IP, uncovering the open ports and exposed services. 

![Screenshot (512)](https://github.com/user-attachments/assets/0492a8d2-dffb-455e-b1ff-b34bb1f00dd1)

After running an Nmap scan, I found two open port: SSH(22) and HTTP(80).
Also added the target IP to /etc/hosts to access the website directly.

### Directory search

I used `dirsearch` to search for directories but I didn't find any userful

![Screenshot (513)](https://github.com/user-attachments/assets/8cabd3d2-df0a-4ebb-b6b0-dc5f066ea1be)

I employed `ffuf` to scan for subdomains on `permx.htb`, revealing the existence of `lms.permx.htb`. 

![Screenshot (514)](https://github.com/user-attachments/assets/ead91c67-47b7-432d-8983-d192046cb9f6)

After adding lms.permx.htb to /etc/hosts, I navigated to the website for further exploration.

![Screenshot (515)](https://github.com/user-attachments/assets/b8e50318-e740-442a-9fae-839e7885e862)

Then I searched for vulnerabilities in chamilo lms and came across CVE-2023-4220, Chamilo LMS Unauthenticated Big Upload File RCE PoC.

![Screenshot (516)](https://github.com/user-attachments/assets/dc4783fd-316f-45a2-8b22-f935b4f3ccce)

Cloned the repository to my machine and installed the dependencies using pip.
Checked if the target was vulnerable by trying to access the /main/inc/lib/javascript/bigupload/files/ endpoint.

`python3 main.py -u http://example.com/chamilo -a scan`

It was indeed vulnerable. I tried to get a reverse shell.
First of all, to get a reverse shell, I started a listener on port 4321 by using netcat.

![Screenshot (517)](https://github.com/user-attachments/assets/81d4077c-b46f-4588-b8c0-43bdb2ecffe5)

And voils, we got a shell!

![Screenshot (518)](https://github.com/user-attachments/assets/f4912a21-43a9-4869-b045-469d7c83903d)

I switched my directory to /var/www/chamilo to make it easier to perform a find operation. I located two configuration.php files and reviewed the content of both. Upon examining them, I discovered that app/config/configuration.php contained some important credentials—exactly what we were looking for.

![Screenshot (519)](https://github.com/user-attachments/assets/169f2ade-402a-4be3-828e-89221567fcf7)

Checked /etc/passwd file’s content, found 2 shell users. One is root and the other one is mtz. Logged in as user mtz with using the password we found before by using ssh.
We successfully logged in via SSH as the mtz user, allowing us to retrieve the user flag!

![Screenshot (521)](https://github.com/user-attachments/assets/2610ab6c-ad1a-4853-bc4d-609532d2185f)

## Privilege Escalation
I used sudo -l to check for any files with sudo privileges that could help in privilege escalation. I found /opt/acl.sh, which can modify permissions on files in the mtz user’s home directory, though it wasn’t writable.

![Screenshot (522)](https://github.com/user-attachments/assets/24c7d98c-b3e9-4b43-ab72-722e92b8dd63)

To exploit this, I created a symbolic link to /etc/passwd and used acl.sh to grant read and write permissions to the symlink for the mtz user. Then, I edited the symlink (and thus /etc/passwd) to give root3 user privileges.

Finally, I switched to root3 with su root3 and gained root access, allowing us to retrieve the root flag from the /root directory!

![Screenshot (523)](https://github.com/user-attachments/assets/173b023e-934a-4081-bacc-3fa622948ca0)

Mission accomplished!


* * *


<footer>
    <p>&copy; 2024 Brenda. All rights reserved.</p>
  </footer>



















