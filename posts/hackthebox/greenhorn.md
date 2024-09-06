---
layout: default
title : brenda - skyfall HTB Writeup
---

![Screenshot (572)](https://github.com/user-attachments/assets/e1933f88-858c-4bf9-adb5-a3c1def5b53a)

`Greenhorn` is an easy machine in HTB.

### Recon
### Nmap scan

Deployed an Nmap scan on the target IP, uncovering the open ports and exposed services. 

![Screenshot (557)](https://github.com/user-attachments/assets/f68acdce-e23d-4b71-ad0f-8dbd67146f23)

After running an Nmap scan, I found two open port: SSH(22), HTTP(80) and http UPnP(3000). Port 3000 seems to return a web page under construction..
Also added the target IP to /etc/hosts to access the website directly.

### Directory search

I used `dirsearch` to search for directories. I found a login page under /login.php, but I don’t have a password yet.

![Screenshot (558)](https://github.com/user-attachments/assets/b4e0e766-069b-4cb7-a6a2-1e0058f4918e)

## User Flag

On port 80 I find a website called Greenhorn, which is provided via Pluck with version 4.7.18.

![Screenshot (575)](https://github.com/user-attachments/assets/056acf4e-7ac0-448a-8977-8a5755ac3103)

Navigated to /login.php page 

![Screenshot (576)](https://github.com/user-attachments/assets/d4971cb3-0ca8-4fc0-b1a7-c8b5395c3c10)

I discovered a Gitea instance running on port 3000 with registration enabled, so I went ahead and signed up.

![Screenshot (577)](https://github.com/user-attachments/assets/c2c69ba2-a931-440c-b1ff-b7f0ade593fe)

I came across a repository named "GreenAdmin" and decided to take a closer look at its files.

![Screenshot (578)](https://github.com/user-attachments/assets/fcd726f9-695f-42ee-8f1e-77d8bc191d18)

I found a hash within the repository.

![Screenshot (579)](https://github.com/user-attachments/assets/da42227d-ef60-4502-93d7-4c3436745e0b)

I used Hashcat to successfully crack the hash.

![Screenshot (580)](https://github.com/user-attachments/assets/b5b72ae9-f05c-4508-8e30-55404a1f2c50)

I then used the password to log in to the Plug website. Plug supports extensions through PHP-based modules, which can be uploaded and installed via the admin page. My next step was to download the Greenhorn repository from Gitea.

![Screenshot (581)](https://github.com/user-attachments/assets/4729ac31-429c-45c2-aac2-93d37b68587c)

![Screenshot (559)](https://github.com/user-attachments/assets/5332d4ce-57c9-47eb-b5cc-3ba0abb1350a)

In the cloned repository history, I copied the "contactform" module and renamed it to "exploit." Within the folder, I changed the filename "contactform.php" to "exploit.php" and "contactform.site.php" to "exploit.site.php." I then replaced the content of "exploit.php" with a PHP reverse shell.

![Screenshot (582)](https://github.com/user-attachments/assets/2e0e80a6-209c-4595-97aa-7446f6c12ef4)

Since only ZIP files are allowed for uploads, I compressed the "exploit" folder into a ZIP file.

![Screenshot (560)](https://github.com/user-attachments/assets/ac3a7921-5b65-4dd4-80fe-b5d991ed330a)

I then select 'exploit.zip' and upload it.

![Screenshot (584)](https://github.com/user-attachments/assets/7b454a9f-4c7b-4f92-a9e1-1273f2fa66b9)

![Screenshot (585)](https://github.com/user-attachments/assets/b6f9ac03-53a9-40fd-abd1-08c38ea1ebf5)


Soon after, we received a reverse shell.

![Screenshot (561)](https://github.com/user-attachments/assets/d74a1437-2e08-475a-b3ed-53dd6e846d3d)

Under the "/home" directory, there are two folders named "git" and "junior." I located the user.txt file in the "junior" folder, which contained the user flag. In the home directory of the user "junior," there is also a file named “Using OpenVAS.pdf.” I then used Python to start an HTTP server on the victim machine, allowing me to download the PDF to my own machine.

![Screenshot (562)](https://github.com/user-attachments/assets/a08b9669-831d-4598-888d-3b0f571bc49b)

On my machine:

![Screenshot (565)](https://github.com/user-attachments/assets/54e7484e-bf38-4ff8-a7fa-a0d71a1fc267)

## Root Flag

Here is the PDF file, with the password obscured.

![Screenshot (569)](https://github.com/user-attachments/assets/56b41295-820e-40c4-9442-d875f5b838d5)

I used the PDF24 tool to extract the image from the PDF.

![Screenshot (583)](https://github.com/user-attachments/assets/9b0c8759-bd72-43bd-a9cf-d135d41cc2bf)

We obtained the following image.

![Screenshot (567)](https://github.com/user-attachments/assets/37955d72-1d37-4e43-885d-1ebaef08b345)

We can use the "Depix" tool to recover the image. I cloned the "Depix" repository using git clone and then ran the Depix tool with the necessary parameters.

![Screenshot (566)](https://github.com/user-attachments/assets/e4e18b38-a26e-45a8-b991-aa432d7697ef)

The output is quite usable. Although it may not be perfect, it is readable. The password is: "sidefromsidetheothersidesidefromsidetheotherside."

![Screenshot (568)](https://github.com/user-attachments/assets/0163ba90-4402-4b9d-8136-4e0f57919b51)

With this password, we can switch to the "root" user and access the root.txt file.

![Screenshot (573)](https://github.com/user-attachments/assets/8a6e8f43-b089-49ca-97bd-b3318dd7bf1d)

![Screenshot (574)](https://github.com/user-attachments/assets/36793276-0564-4cb3-a8aa-1b4fa8260f0d)

That's it!!

* * *


<footer>
    <p>&copy; 2024 Brenda. All rights reserved.</p>
  </footer>
















































