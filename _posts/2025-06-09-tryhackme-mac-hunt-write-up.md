---
title: TryHackMe Mac Hunt Write-Up Walk Through
date: 2025-06-09 07:00:00 +0200
categories: [TryHackMe, CTF, TryHackMe Write Up]
tags: [Mac Hunt, macOS, DFIR, TryHackMe Mac Hunt Walkthrough, Incidentresponse, tryhackme, ctf]
description: "Utilize your macOS investigation skills to reveal the mystery behind a compromise."
comments: false
media_subpath: /assets/img/
image:
  path: tryhackme-mac-hunt.png
  alt: TryHackMe Mac Hunt
---

# Investigating a macOS Compromise: Mac Hunt Walkthrough (Part 1)

In this TryHackMe room Mac Hunt, we investigate how Jake's macOS system was compromised through a fake job offer. Let's solve the first three questions to uncover the attack chain.

**[You can follow YouTube video walk through here](https://youtu.be/E2HV1OCXGiE)**

## 🛠️ Scenario Recap
Jake had gained some good knowledge and skills in the game development field. So, he decided to enter the industry through a decent job and upgrade his finances. Little did he know that there were many fake recruiters in search of people looking for jobs. These fake recruiters lure the victim through attractive jobs to achieve their objectives, often to compromise the victim's machines and use them for malicious purposes. Having conventionally overlooked cyber security, Jake fell prey to such an attack. A well-crafted phishing attack with a promising job offer compromised his Mac machine.

**[You can follow YouTube video walk through here](https://youtu.be/E2HV1OCXGiE)**

## 🔐 Q1: Most Recently Accessed Folder

To find the most recently accessed folder, we examine macOS artifacts, specifically the Finder preferences `plistutil -p Library/Preferences/com.apple.finder.plist`:

```console
root@tryhackme:/home/ubuntu/mac/root/Users/jake# plistutil -p Library/Preferences/com.apple.finder.plist
{
  "FXDesktopVolumePositions": {
    "GUEST TOOLS_-0x1.d27e44p+29": {
      "xRelative": -65,
      "ScreenID": 0,
      "AnchorRelativeTo": 1,
      "yRelative": 65
    }
  },

[REDACTED]

   {
      "file-bookmark": <626f6f6b 98020000 00000410 30000000 ... 04 00000000 000000>,
      "name": "Downloads"
    },
    {
      "file-bookmark": <626f6f6b 98020000 00000410 30000000 ... 04 00000000 000000>,
      "name": "Documents"
    },
    {
      "file-bookmark": <626f6f6b 6c030000 00000410 30000000 ... 04 00000000 000000>,
      "name": "GUEST TOOLS"
    }
  ],
```
This reveals the recently accessed folders, with the most recent one being our answer.

**[You can follow YouTube video walk through here](https://youtu.be/E2HV1OCXGiE)**

## ⚙️ Q2: Attacker's Delivery Platform

The attacker likely delivered the malicious document via a social platform. We check Safari's download history `plistutil  -p Library/Safari/Downloads.plist `:

```console
root@tryhackme:/home/ubuntu/mac/root/Users/jake# plistutil  -p Library/Safari/Downloads.plist 
{
  "DownloadHistory": [
    {
      "DownloadEntryProgressTotalToLoad": 3915,
      "DownloadEntryProgressBytesSoFar": 3915,
      "DownloadEntryPath": "/Users/jake/Downloads/MeetMeLiveInstaller.pkg",
      "DownloadEntryDateAddedKey": 2025-04-30 08:53:46 +0000,
      "DownloadEntryRemoveWhenDoneKey": false,
      "DownloadEntryShouldUseRequestURLAsOriginURLIfNecessaryKey": false,
      "DownloadEntryProfileUUIDStringKey": "DefaultProfile",
      "DownloadEntryDateFinishedKey": 2025-04-30 08:53:49 +0000,
      "DownloadEntryURL": "http://files.techthm.careers.thm:8080/MeetMeLiveInstaller.pkg",
      "DownloadEntrySandboxIdentifier": "E226AE37-B1E0-4273-B615-D742DCE5AC46",
      "DownloadEntryBookmarkBlob": <626f6f6b c4020000 00000410 30000000 ... 04 00000000 000000>,
      "DownloadEntryIdentifier": "9AF3855A-649A-4BFE-844E-7DCEC97A2B19"
    },
    {
      "DownloadEntryProgressTotalToLoad": 48401,
      "DownloadEntryProgressBytesSoFar": 48401,
      "DownloadEntryPath": "/Users/jake/Downloads/TechTHM Interview Instructions ? Content Writer.pdf",
      "DownloadEntryDateAddedKey": 2025-04-30 08:48:27 +0000,
      "DownloadEntryRemoveWhenDoneKey": false,
      "DownloadEntryShouldUseRequestURLAsOriginURLIfNecessaryKey": false,
      "DownloadEntryProfileUUIDStringKey": "DefaultProfile",
      "DownloadEntryDateFinishedKey": 2025-04-30 08:48:29 +0000,
      "DownloadEntryURL": "https://www.linkedin.com/dms/prv/vid/v2/D4D06AQGm4JKZe0_YOg/messaging-attachmentFile/B4DZaFtOpBH4AI-/0/1745999948823?m=AQLwEV_pZVeJggAAAZaF4f1sF3cQqoqdWb6NBttKAHbEUTE25fLpzLG5LiE&ne=1&v=beta&t=SBiD-QsKelAXCBMiHy1dRdm001vHvuqqkKADE9dBV10",
      "DownloadEntrySandboxIdentifier": "ED8D283A-8AB7-40D8-B9F1-944DE44B7CA0",
      "DownloadEntryBookmarkBlob": <626f6f6b e0020000 00000410 30000000 ... 04 00000000 000000>,
      "DownloadEntryIdentifier": "B0A2342C-E7D7-45D8-B3B2-247B916EA283"
    }
  ]
}

```

This shows downloads from platforms like LinkedIn, revealing where the attacker contacted Jake.


**[You can follow YouTube video walk through here](https://youtu.be/E2HV1OCXGiE)**

## Q3: Malicious Download Link

Inside Jake's Downloads folder, we find a suspicious PDF. Opening it reveals a hyperlink `Click here to download MeetMeLive!`:

This crafted link leads to the malicious application.

<img src="tryhackme-hun-jake-job-offer-pdf.png"  alt="TryHackMe Mac Hunt Jake PDF Job Offer" >


# 🎥 Want to Go Deeper? Watch the Full Walkthrough

Watch the complete step-by-step solution on my 

👉 🔗  **[You can follow YouTube video walk through here](https://youtu.be/E2HV1OCXGiE)**


<img src="Mac-Hunt-TryHackMe-Forensics-Walkthrough-macOS-Incident-Response-Phishing-Attack-YouTube.png"  alt="TryHackMe Mac Hunt YouTube Video" >

Check out my **[full video walkthrough](https://youtu.be/E2HV1OCXGiE)** on my YouTube channel for step-by-step guidance:

Stay tuned for more macOS forensic investigations! 🔍💻

