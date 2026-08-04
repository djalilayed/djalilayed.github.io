---
title: TryHackMe Hacker Holidays - The Byte Lotus - Write up -
date: 2026-07-30 06:00:00 +0200
categories: [TryHackMe, CTF,TryHackMe Write Up]
tags: [tryhackme,ctf,Cybersecurity,Byte Lotus,Hacker Holidays]
description: "Beginner-friendly CTF from TryHackMe"
comments: false
media_subpath: /assets/img/
image:
  path: tryhackme-hacker-holidays.jpg
  alt: TryHackMe Hacker Holidays
---

## Hacker Holidays: Welcome to The Byte Lotus:

*A five-star resort with a zero-star security posture. 14 days of free hacking challenges drop daily from 27 July. Beginners especially welcome, the hotel certainly didn't hire experts...*

*Event link* **[Hacker Holidays](https://tryhackme.com/hackerholidays)**

This is free CTF from TryHackme from Monday 27th, for 14 days, Every day at 4PM GMT.

Below are my full YouTube videos walk through for the challenges.

Video playlist: **[TryHackMe Hacker Holidays - The Byte Lotus Hotel](https://www.youtube.com/playlist?list=PLcJYIZq2TVhM)**

## Day 9: CryptoCabana

*Room Link:* **[TryHackMe CryptoCabana](https://tryhackme.com/room/hh-cryptocabana-f81cac95)**

*YouTube Vide Walk Through:* **[TryHackMe CryptoCabana - Hacker Holidays](https://youtu.be/-6vUxb-t4Rg)**

*He never signed the transfer. The place he stashed his secret wasn't as sealed as promised.*

**Cloud - Azure - Storage - Key Vault**

### Tasks:

<ul>
  <li>Pull apart what the kiosk hands out for free before you've even clicked anything.</li>
  <li>Follow that trust somewhere the kiosk's own page never once points you.</li>
  <li>Somewhere in there is a second, more valuable set of keys — and a vault that won't give up the real values on the first ask.</li>  
</ul>

## Day 8: Towel on the Sunbed

*Room Link:* **[TryHackMe Towel on the Sunbed](https://tryhackme.com/room/hh-towelonthesunbed-61271709)**

*YouTube Vide Walk Through:* **[TryHackMe Towel on the Sunbed - Hacker Holidays](https://youtu.be/iwIhpo9MLVA)**

*Ponzi set his towel down for one 24-hour reward claim. He came back to find the sunbed had been "claimed" three times over while he wasn't looking.*

**Web - Boot2root**

### Tasks:

<ul>
  <li>Create a guest account and explore Ponzi's daily reward mechanism.</li>
  <li>Work out exactly what's standing between you and Whale Vault status.</li>
  <li>Find your way past it and retrieve the flag from the vault.</li>  
</ul>

## Day 7: Do Not Disturb

*Room Link:* **[TryHackMe Do Not Disturb](https://tryhackme.com/room/hh-donotdisturb-84a45644)**

*YouTube Vide Walk Through:* **[TryHackMe Do Not Disturb - Hacker Holidays](https://youtu.be/ruWFbw3ALT0)**

*Sign's on the door. Room's active. You have access you were never given, and so does he.*

**Web - Boot2root**

### Tasks:

<ul>
  <li>Find the user flag</li>
  <li>Find the root flag</li>
</ul>



## Day 6: Overheard at Breakfast

*Room Link:* **[TryHackMe Overheard at Breakfast](https://tryhackme.com/room/hh-overheardatbreakfast-6f01793c)**

*YouTube Vide Walk Through:* **[TryHackMe Overheard at Breakfast - Hacker Holidays](https://youtu.be/iy70uGRU_sQ)**

*Two strangers. One conversation. One profile they never meant to reveal.*

**OSINT - Social Media - Hashing**

### Tasks:

<ul>
  <li>Analyze the provided conversation for identifying details</li>
  <li>Extract the relevant clues</li>
  <li>Locate the hidden account</li>
  <li>Submit the flag</li>
</ul>

## Day 5: Beach Bar

*Room Link:* **[TryHackMe Beach Bar](https://tryhackme.com/room/hh-beachbar-d849f7f7)**

*YouTube Vide Walk Through:* **[TryHackMe Beach Bar - Hacker Holidays](https://youtu.be/GMBbSoBAVEw)**

*At the Beach Bar, even shell access is complimentary. The jukebox takes requests. Any kind.*

**Web - Boot2root**

### Tasks:

<ul>
  <li>Find the user flag</li>
  <li>Find the root flag</li>
</ul>

## Day 4: Packed Light

*Room Link:* **[TryHackMe Packed Light](https://tryhackme.com/room/hh-packedlight-02e5330c)**

*YouTube Vide Walk Through:* **[TryHackMe Packed Light - Hacker Holidays](https://youtu.be/5ld7tvq_W2Y)**

*Tiny packets. Odd hours. Suspiciously regular. Someone's smuggling out the data equivalent of a hotel towel every night, folded neatly inside traffic that looks ordinary until you decode it.*

**Network Forensics - PCAP Analysis - Cryptography**

### Tasks:

<ul>
  <li>Analyze the provided capture for a covert communication channel.</li>
  <li>Identify where the exfiltrated data is being hidden and reassemble it</li>
  <li>Decode the recovered data and submit the flag</li>
</ul>

## Day 3: Complimentary

*Room Link:* **[TryHackMe Complimentary](https://tryhackme.com/room/hh-complimentary-05e0b604)**

*YouTube Vide Walk Through:* **[TryHackMe Complimentary - Hacker Holidays](https://youtu.be/aXMNQxlafZY)**

*Install the free app and it hands your phone a set of cloud keys, the same set it hands everyone. They're read-only, but read-only of every guest's contacts, location, and passwords, not just Lambo's. She gave consent. Technically.*

**Cloud - AWS - Cognito - IAM Misconfiguration**

### Tasks:

<ul>
  <li>Track down AWS the mechanism issuing you credentials behind the scenes.</li>
  <li>Use those credentials to dump more than your own record from the app's DynamoDB table.</li>
  <li>Retrieve the flag from another guest's data.</li>
</ul>

## Day 2: Room 404

*Room Link:* **[TryHackMe Room 404](https://tryhackme.com/room/hh-room404-804573bf)**

*YouTube Vide Walk Through:* **[TryHackMe Room 404 - Hacker Holidays](https://youtu.be/TPAIahveadw)**

*He booked the quiet room. It's not on the floor plan, not in the brochure, not on any door. But port 8080 is wide open, and the rooms it never lists are the ones worth finding.*

**Web - Directory Enumeration**

### Tasks:

<ul>
  <li>Dump the exposed source code.</li>
  <li>Find the flag.</li>
</ul>

## Day 1: The Concierge Knows Too Much

*Room Link:* **[TryHackMe The Concierge Knows Too Much](https://tryhackme.com/room/hh-theconciergeknows-2d7eb4d9)**

*YouTube Vide Walk Through:* **[TryHackMe The Concierge Knows Too Much - Hacker Holidays](https://youtu.be/91RMmtKipUQ)**

*She knows your name, your room, your coffee order, none of which you told her. Word your next question carefully and she'll also hand over the instructions she was told to keep to herself.*

**AI - Prompt Injection - Social Engineering - Security**

### Tasks:

<ul>
  <li>Work out why VERA already seems to know exactly who you are.</li>
  <li>Figure out what she's protecting - and who she actually trusts.</li>
  <li>Convince her you're someone she trusts, then get her talking. Grab the flag from what she reveals.</li>
</ul>

## Day 0: The Brochure

*Room Link:* **[TryHackMe The Brochure](https://tryhackme.com/room/hh-thebrochure-081f3e36)**

*YouTube Vide Walk Through:* **[TryHackMe The Brochure - Hacker Holidays](https://youtu.be/tgi1LxsJ0I4)**

*The brochure's hero photo has an AI fingerprint. Follow the account that posted it, and the trail doesn't end at the hotel; it ends at someone the hotel never mentioned.*

**AI - Prompt Injection - Social Engineering - Security**

### Tasks:

<ul>
  <li>Analyze the provided image for embedded clues.</li>
  <li>Apply fundamental OSINT techniques to trace the findings.</li>
  <li>Locate the hidden social media account.</li>
  <li>Submit the flag.</li>
</ul>


