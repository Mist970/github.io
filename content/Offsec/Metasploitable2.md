---
title: "CTF 1: Metasploitable 2"
date: 2023-12-03T09:19:02+02:00
tags: ["Offsec", "Ethical_Hacking", "CTFs"]
toc: true
image: "/images/Metasploitable_2/msf.png"
---

Although I have already went over this box in the past & it is pretty old too, I thought it'd be a good start to my CTF series where I'll be doing my best to root a variety of CTFs. 

The main purpose of this series is to point out various aspects of systems one can leverage to gain further access into said systems. As well as this, i'll be detailing the purpose of the many hosted services we will see in upcoming CTFs & ways to protect them from attacks too.

I hope you enjoy!

----------------------------------------------------------------------------------------------------------------------
# The Topology
| Hosts:                  | IPs:          |
|-------------------------|---------------|
| Kali                    | 192.168.10.40 |
| Metasploitable 2 Server | 192.168.20.10 |


## Reconnaissance
After performing a SYN & Version Scan using Nmap, you can see the server's various services listed below as well as their associated ports.  

![nmapscan.jpg](/images/Metasploitable_2/nmapscan.jpg)

> Now theres a lot to unpack here. So lets start off by simply going through what services are open and what they are vulnerable to.

> The Metasploit Framework is a popular tool used in penetration tests to assess the security posture of systems & leverage security vulnerabilities through exploitation.

## Exploitation: FTP
We will be using the msfconsole to tackle our first vulnerable service, FTP. The ftp server which is served by "vsftpd V2.3.4". This specific version of vsftpd has a vulnerability present in the software. 

First off, we will use msf to search for the vulnerability itself. Typing the keyword "vsftpd" does the trick. You can see in the description of the exploit that is produces a backdoor to the system.

The **use** command will allow us to select the Module/exploit.

![msf_vsftpd.jpg](/images/Metasploitable_2/msf_vsftpd.jpg)
**Show Options** is used to inspect the possible configurations that can be done within this script. 

We will need to configure this by setting the **"RHOSTS"** option to the target IP address, in this case, the Metasploitable 2 machine. 

![msf_vsftpd_show_options.jpg](/images/Metasploitable_2/msf_vsftpd_show_options.jpg)

Using the **exploit** command, we will now execute the actual payload. Doing this gives us root access to the Metasploitable 2 machine.  
![msf_vsftpd_exploit.jpg](/images/Metasploitable_2/msf_vsftpd_exploit.jpg)  
Once your in the system as root, you can access sensitive files like the **Shadow** file containing the password hashes for all users on the system. This can assist you in laterally moving across users and possibly other systems too. 

Now this is just one area of this machine we have exploited, however, there's a lot more to uncover. 

But Before we do that, I will attempt to shed some light on the blue team perspective of this attack via the Security Onion SIEM.

### FTP Attack Analysis (SIEM)
So far, we have issued an Nmap scan to see the running services as well as their versions on the vulnerable machine. We have also gained access to the root account by leveraging a vulnerable FTP service. 

Now we shall see all of this within the SIEM. (I will try my best to describe what had happened as I am still getting used to Security Onion as of now)

You can see a lot of alerts. These alerts are around the time frame of our attack. 

The first few alerts are in regards to our Nmap scan. The alert named "GPL DNS named version attempt" shows that the Nmap scan was trying to get information on the version of running services.

![msf_vsftpd_SIEM_Pic_1.jpg](/images/Metasploitable_2/msf_vsftpd_SIEM1.jpg)

The alert I want to dive into is "GPL ATTACK_RESPONSE id check returned root". This alert was triggered when we exploited the FTP server and gained root access to the system.

![msf_vsftpd_SIEM_Pic_2.jpg](/images/Metasploitable_2/msf_vsftpd_SIEM2.jpg)

In the image above, when inspecting this alert further, we see our user input when we gained full access to the system. However, the alert was triggered due to the **id** command which gave us the following information:

uid=0(root) gid=0(root)

Hence triggering the alert.

## Exploitation: Distcc  
**CVE-2004-2687** is a vulnerability that allows one to perform remote code execution on servers that have the "Distributed C Compiler" deamon running on them.

### What is Distcc?
	Distcc stands for "Distributed C Compiler". It allows programmers to compile code using not only the local machine but also all 
    available servers too. 
    
    These servers would utilize their unused processing power to help compile to the code too.
    The compilation takes place through the user's remote commands to the servers. 
    Typically, the user would send the source code and the compilation commands to all the available serves to help compile the code
