---
title: TryHackMe Industrial Intrusion Breach Walk Through
date: 2025-06-26 08:00:00 +0200
categories: [TryHackMe, CTF, TryHackMe Write Up,Industrial Intrusion]
tags: [tryhackme,ctf, robots, ctf, Industrial Intrusion, breach]
description: "This engagement aims to find a way to open the gate by bypassing the badge authentication system."
comments: false
media_subpath: /assets/img/
image:
  path: industrial-intrusion-breach-1.png
  alt: Industrial Intrusion Breach
---

## Breach

This engagement aims to find a way to open the gate by bypassing the badge authentication system.
The control infrastructure may hold a weakness: Dig in, explore, and see if you have what it takes to exploit it.

Be sure to check all the open ports, you never know which one might be your way in!

**[You can follow YouTube video walk through here](https://youtu.be/1sSZb-Vefhs)**

### Initial Enumeration:

First we start with port enumeration to see which ports are open

```
root@ip-10-10-120-31:~# rustscan -a 10.10.202.121
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
\U0001f635 https://admin.tryhackme.com

[~] The config file is expected to be at "/home/rustscan/.rustscan.toml"
[~] File limit higher than batch size. Can increase speed by increasing batch size '-b 1048476'.
Open 10.10.202.121:22
Open 10.10.202.121:80
Open 10.10.202.121:102
Open 10.10.202.121:502
Open 10.10.202.121:1880
Open 10.10.202.121:8080
Open 10.10.202.121:44818
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")
```

Port `1880` is running `Node-Red` a Low-code programming for event-driven applications
Port `8080` is running `OpenPLC` a multi-hardware Programmable Logic Controller Suite based on Beremiz IDE

**[You can follow YouTube video walk through here](https://youtu.be/1sSZb-Vefhs)**

<img src="Node-Red.png"  alt="Node Red" >


### Opening the Gate

Checking port 80 we can see the gates we suppose to open in this task:

<img src="Gate-Status-Monitor.png"  alt="Gate Status Monitor" >



# Want the Full Walkthrough?

Check out my **[full video walkthrough](https://youtu.be/1sSZb-Vefhs)** on my YouTube channel for step-by-step guidance:

**[You can follow YouTube video walk through here](https://youtu.be/1sSZb-Vefhs)**

<img src="youtubue-video-Industrial-Intrusion-CTF.png"  alt="TryHackMe: Industrial Intrusion CTF (Breach Task) | Node-RED | Industrial Intrusion CTF" >

**[You can follow YouTube video walk through here](https://youtu.be/1sSZb-Vefhs)**


