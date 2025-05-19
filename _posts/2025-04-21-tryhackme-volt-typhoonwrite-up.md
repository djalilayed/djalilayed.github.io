---
title: TryHackMe Volt Typhoon Write-Up Walk Through
date: 2025-05-19 07:00:00 +0200
categories: [TryHackMe, CTF, TryHackMe Write Up]
tags: [Volt Typhoon, APT Investigation, APT Investigation, TryHackMe Volt Typhoon Walkthrough, Splunk, Volt Typhoon TryHackMe, ctf]
description: "Investigate a suspected intrusion by the notorious APT group Volt Typhoon."
comments: false
media_subpath: /assets/img/
image:
  path: tryhackme-volt-typhoon.png
  alt: TryHackMe Volt Typhoon
---

# Investigating Volt Typhoon APT Attack: Initial Access & Execution Analysis (TryHackMe)

In a world where cyber threats grow more sophisticated by the day, APT groups like Volt Typhoon have become notorious for their stealthy operations and deep network penetration techniques. In the TryHackMe: Volt Typhoon challenge, you're tasked with investigating a simulated but realistic attack that mirrors the tactics of this high-level threat actor.

In this article, we'll guide you through the Initial Access and Execution stages of the Volt Typhoon incident and provide concrete answers to help you progress. If you're stuck or need a full walkthrough, we've also linked a complete YouTube video tutorial at the end.

**[You can follow YouTube video walk through here](https://youtu.be/Pl2Bnza_8cE)**

## 🛠️ Scenario Recap
Your SOC team has flagged unusual activity tied to Volt Typhoon, an APT group known for targeting government and critical infrastructure. As the security analyst, your mission is to:

Trace the attack timeline.
Uncover compromised accounts.
Analyze logs from tools like Splunk and ADSelfService Plus.
Understand execution tactics like NTDSUtil and data exfiltration.

**[You can follow YouTube video walk through here](https://youtu.be/Pl2Bnza_8cE)**

Check the sourcetype associated with the logs by running:
```console
index=* | stats count by sourcetype, source
```

<img src="TryHackMe-Volt-Typhoon-Splunk.png"  alt="TryHackMe Volt Typhoon - Splunk" >

**[You can follow YouTube video walk through here](https://youtu.be/Pl2Bnza_8cE)**

## 🔐 Phase 1: Initial Access

The attacker starts by compromising a user account — a common tactic in targeted attacks. Your first task is to identify when this occurred.

Q1: At what time (ISO 8601 format) was Dean's password changed?

We will use this query (ADSelfService Plus logs):

```console
index="*" sourcetype=adss  username="dean-admin" | table _time, timestamp, ip_address, action_name, status | sort -_time
```

<img src="TryHackMe-Volt-Typhoon-Splunk-ADSelfService-Plus-logs.png"  alt="TryHackMe Volt Typhoon ADSelfService Plus logs- Splunk" >

➡️ Inspecting the ADSelfService Plus logs, we find that Dean’s password was changed at:

`2024-03-[Redacted]:10:22`

This marks the beginning of the intrusion. The attacker now has access to the environment using legitimate credentials.

Q2: What is the name of the new administrator account that was created?
➡️ Shortly after the takeover, a new account appears in the logs:
✅ voltyp-admin

**[You can follow YouTube video walk through here](https://youtu.be/Pl2Bnza_8cE)**

## ⚙️ Phase 2: Execution

Once inside the network, the attacker begins information gathering on the local infrastructure.

Q3: What command does the attacker run to find local drive information on server01 & server02?
➡️ The logs show the use of the following command:

```console
wmic /node:server01, server02 logicaldisk get caption, [Redacted], size, volumename
```
Q4: What password does the attacker set on the archive after compressing the AD database?
➡️ The attacker uses the password:
✅ `d5a[Redacted]@5t3r`

This is applied during the compression phase after exfiltrating the ntds.dit file, indicating data theft and preparation for external access.

**[You can follow YouTube video walk through here](https://youtu.be/Pl2Bnza_8cE)**

# 🎥 Want to Go Deeper? Watch the Full Walkthrough

If you’re working through this TryHackMe room and want a full explanation of each phase — including Persistence, Defense Evasion, Credential Access, and C2 Communications — check out my step-by-step video guide here:

👉 🔗  **[You can follow YouTube video walk through here](https://youtu.be/Pl2Bnza_8cE)**


<img src="TryHackMe-Volt-Typhoon-youtube-video.png"  alt="TryHackMe Volt Typhoon YouTube Video" >

Check out my **[full video walkthrough](https://youtu.be/Pl2Bnza_8cE)** on my YouTube channel for step-by-step guidance:



