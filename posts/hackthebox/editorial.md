---
layout: default
title : brenda - skyfall HTB Writeup
---

![Screenshot (524)](https://github.com/user-attachments/assets/498a8563-38ec-48e2-a4c3-cea8bd1a0180)

`Editorial` is an easy machine in HTB.

### Recon
### Nmap scan

Deployed an Nmap scan on the target IP, uncovering the open ports and exposed services. 

![Screenshot (549)](https://github.com/user-attachments/assets/75bb3fe7-9bc3-407d-8573-10672c83333a)

After running an Nmap scan, I found two open port: SSH(22) and HTTP(80).
Also added the target IP to /etc/hosts to access the website directly.

### Directory search

I used `dirsearch` to search for directories and found one /upload

![Screenshot (542)](https://github.com/user-attachments/assets/39c717a3-462a-45f8-b05f-51ab7a244224)

## User Flag

As attackers, we’re always drawn to upload pages because they provide a range of potential attack vectors—it's that simple!

![Screenshot (543)](https://github.com/user-attachments/assets/0292e5f2-aefc-4494-8a70-1b37b45edb5a)

Inspected the prview button on burpsuite I observed a request is being sent to the server as shown below and got the static image on visiting.

![Screenshot (534)](https://github.com/user-attachments/assets/8948fd8b-377c-4a0e-81c3-eeaa0056905c)

This is a classic indication of SSRF. We've discovered how to get our first foothold into the system, or at least the path toward it.
SSRF enabled the attacker to reroute and manipulate the server toward an external server, even directing it to our own server.

Since we saw that only two ports, 22 and 80, were open, we might have redirected localhost, potentially revealing an undiscovered route. This technique is known as SSRF Out-of-Bound.

![Screenshot (550)](https://github.com/user-attachments/assets/953cb2d4-dfdf-4d9a-adc5-35a77316d590)

Pretty much the same, got a jpeg endpoint in the response.

![Screenshot (535)](https://github.com/user-attachments/assets/27c15aff-d440-412a-906f-d6e015e168cd)

Next step, was to use Intruder to brute force the port. Set the payload type as Numbers with From 1 TO 65535 and Step 1. And ATTACK!

![Screenshot (551)](https://github.com/user-attachments/assets/c3d48fc5-f873-4de6-ad41-dcb6b5a4cb70)

Filtered out the results which had the usual jpeg endpoint in the response and got the 5000 port as the different with a different length and endpoint in the response.

![Screenshot (536)](https://github.com/user-attachments/assets/afe4f720-ea9d-46e8-915f-86d5ac6a4610)

Sent request on port 5000 and got

![Screenshot (537)](https://github.com/user-attachments/assets/73214d9a-03b8-4cf9-b883-d9c0638d44e4)

![Screenshot (553)](https://github.com/user-attachments/assets/23e1b32a-8429-48f9-8cae-448ac48562d1)

On checking one of end point.
/api/latest/metadata/messages/authors

![Screenshot (539)](https://github.com/user-attachments/assets/99ed9114-9a4d-4f43-bcd2-ad448e55a393)

Got a username and a password

![Screenshot (540)](https://github.com/user-attachments/assets/e8c0927d-1f5f-40fd-9694-120d738b610d)

Lets try to ssh as a dev. It was successful and got user flag.

![Screenshot (544)](https://github.com/user-attachments/assets/0f09f19a-0c09-4b6f-a3c4-d9178ceb518a)

## Root Flag

There was an apps directory there, looked at it. It might have .git or .docker in it and guess what I found .git. 

![Screenshot (545)](https://github.com/user-attachments/assets/0733527f-3d37-43db-b6b3-83c617059d2d)

We went ahead and ran the git log command, then checked different commits using git show "commit". After trying all of them, we found an interesting one.

![Screenshot (546)](https://github.com/user-attachments/assets/c1fcb9e6-3790-47b8-8cc3-22f756137b3d)

In this, they had changed the prod credentials to the dev credentials that we had found earlier. We then SSHed into the prod user.

![Screenshot (554)](https://github.com/user-attachments/assets/f577a62a-337b-4052-97ea-92840eafc669)

Since there were no direct leads, I tried the sudo -l to command for priv escalation.

![Screenshot (555)](https://github.com/user-attachments/assets/057ba57a-a948-4da3-808c-1e7414327343)

This means that with sudo privileges we can run python3 command above mentioned python code. Let’s see what it is.

![Screenshot (556)](https://github.com/user-attachments/assets/aa29c310-1ea5-4ed5-86e7-91aeab18db1a)

While analyzing the code files and packages using the pip3 list command, we discovered a vulnerable package: GitPython 3.1.29, which was affected by a remote code execution (RCE) vulnerability, listed as CVE-2022-24439.
Let's try exploit it.

![Screenshot (547)](https://github.com/user-attachments/assets/3e9e4be8-785a-49aa-b9d1-455af6dedada)

Since we’re seeing errors related to Python 3 execution, it indicates that the Python file was run as the root user.
Now, let’s modify it to retrieve the contents of root.txt from the /root directory.

![Screenshot (548)](https://github.com/user-attachments/assets/9c4d9418-755e-4677-9d03-6cc9ad1089d5)

And thats how it's done!!!!


* * *


<footer>
    <p>&copy; 2024 Brenda. All rights reserved.</p>
  </footer>











































