---
title: "My Homelab"
date: 2023-12-03T09:19:02+02:00
tags: ["Homelab", "Projects"]
toc: false
image: "/images/Homelab/HTopology.png"
---
## My Homelab Setup:
The topology above is supposed to be my homelab, although my artistic efforts are questionable to say the least, I hope it gives you an idea as to what technologies I'll be dealing with. Credit goes to [Marko Andrejić](https://www.linkedin.com/in/markoandrejic93/overlay/about-this-profile/) , from whom I drew inspiration to build this lab.



This homelab is my attempt at various cyber operations like Offensive Security & DFIR.

_My current Cyber Security Homelab configuration:_

**Corporate LAN (Critical servers):**
* Active Directory 2019 Server + Windows Hosts
* Metasploitable 2 (Represents a vulnerable server in an org)
* _Any other Vulnerable server would be added into this VLAN_
* Other servers & databases shall be added in the future

**Corporate WAN (Fake Internet):**
* Blackarch/Kali Machine (Threat actor's presence within the "Intranet" of an org)

**DFIR & SIEM:**
* Tsurugi
* Security Onion (Mainly Sucirata & Wazuh for SIEM as I'm still getting used to SO)

**Malware Analysis:**
* REMnux
* Flare OS

The main goal of this Homelab is to gain experience in many areas in cyber security like malware analysis, working with a SIEM, Pentesting & general IT knowledge of AD to maintain good security practices.

All my CTF challenges will be done within this homelab environment too.