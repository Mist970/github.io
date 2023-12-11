---
title: "My Homelab"
date: 2023-12-03T09:19:02+02:00
tags: ["Homelab", "Projects"]
toc: true
---
# Topology:

## Networks & Hosts:
This homelab is my attempt at simulating various parts of an organisation's network infrastructure. I have seperated the various areas into VLANs & used PfSense to permit and deny certain areas from connecting to others too. 

This is my current configuration:

Corporate LAN (Critical servers):
* Active Directory 2019 Server + Windows Hosts
* Metasploitable 2 (Represents a vulnerable server in an org)
* *Any other Vulenrable server would be added into this VLAN*
* Other servers & databases shall be added in the future

Corporate WAN (Fake Internet):
* Blackarch/Kali Machine (Threat actor's presence within the "Internet" of an org)

DFIR & SIEM:
* Tsurugi
* Security Onion (Mainly Wazuh for SIEM as I'm still getting used to SO)

Malware Analysis:
* REMnux
* Flare OS

The main goal of this Homelab is to gain experience in many areas in cyber security like malware analysis, working with a SIEM, Pentesting & general IT knowledge of AD to maintain good security practices.

All my CTF challenges will be done within this homelab envrionemnt too.