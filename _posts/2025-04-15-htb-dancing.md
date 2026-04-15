---
title: "HTB Dancing — SMB Didn’t Even Try"
date: 2026-04-15
categories: [HTB, Writeup]
tags: [htb, ctf, beginner, nmap,smb]
---

Dancing. Still Starting Point. Still very easy. This one is about SMB — and honestly, it just handed over the flag.

## What even is this?

Another Tier 0 machine. Spawn → get IP → figure out how to get in.
This time the focus is **SMB (Server Message Block)**.

SMB is what Windows uses to share files over a network.  
If you’ve ever accessed something like:

\\192.168.x.x\folder

that’s SMB.

Which means:
- your own Windows machine already has SMB  
- it can connect to other shares (client)  
- it can host shares (server, if enabled)  

So yeah — this isn’t some niche thing. It’s everywhere.

## Step 1 — What’s open?

Start with a scan:

```bash
nmap -sV 10.129.84.46
```

So now the situation is, this machine is sharing files over the network.

## Step 2 — Let’s list the shares

Use:

```bash
smbclient -L 10.129.84.46
```

> `-L` = list shares

Think of shares as folders that the machine is exposing to the network

At this point, we’re basically asking - what folders can I see from outside?

## Step 3 — Can we access them?

Tried connecting:

```bash
smbclient //10.129.84.46/<sharename>
```

And it just… worked.

No credentials. No prompt. Just in.

Which basically means the share is open to anyone

## Step 4 — Look around

Inside:

```bash
ls
```

Found: flag.txt

Download it:

```bash
get flag.txt
```

Open it locally:

```bash
cat flag.txt
```

Done.

## What did I actually learn?

SMB can expose files directly if not configured properly.
No exploit needed. Just:

find the service
check access
grab what’s there

Also, this isn’t just HTB stuff.

Your own Windows system has SMB shares too.
You can check them with:

```bash
net share
```

Some are default (like C$, ADMIN$), some are user-created.

So the real takeaway is - if sharing is enabled and not secured, anyone on the network might access it.

## Final thought

This box wasn’t about breaking in, it was about noticing the door is already open, and that happens more often than you’d think.


— Aarushi, somewhere in the void 🖤
