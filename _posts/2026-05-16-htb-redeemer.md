---
title: "HTB Redeemer — Redis Said Come In, No Password Needed"
date: 2026-05-16
categories: [HTB, Writeup]
tags: [htb, ctf, ftp, beginner]
---

Redeemer. Starting Point. Very Easy. This one's about Redis - a database that just... forgot to lock its door.

## What even is this?

Redis is a database, but not the boring SQL kind. It stores data in RAM instead of disk, which makes it crazy fast. Think of it like your browser's autofill — quick, temporary, always ready.

It works like:
key → value
"username" → "admin"
"flag" → "HTB{...}"

Real systems use it for caching, sessions, quick lookups. The web server talks to Redis quietly in the background - you as a user never touch it directly.

*But in HTB? Sometimes it's just sitting there. Wide open. No password. Waving at you.*

## Step 1 — Scanning (with a plot twist)

Started with the usual:

```bash
nmap -Pn -sV 10.129.94.126
```

Nothing. Crickets. Not a single port showed up.

Default nmap only scans the top 1000 ports - Redis runs on **6379**, which didn't make the guest list. So, full scan:

```bash
nmap -p- -Pn --min-rate 1000 -T4 10.129.94.126
```

`-p-` = scan all 65535 ports  
`--min-rate 1000` = send at least 1000 packets per second  
`-T4` = aggressive speed, still safe  

Done in under a minute. **Port 6379 open — Redis.**

> Pro tip - whenever default nmap gives you nothing, go full port scan with speed flags. Saves your life (and your time).

## Step 2 — Redis Tools

Redis wasn't installed by default on Kali, so:

```bash
sudo apt update  
sudo apt install redis-tools
```

This gives you `redis-cli` — the tool to talk directly to a Redis server.


## Step 3 - Connecting

```bash
redis-cli -h 10.129.94.126
```

Quick check if it's alive:

```bash
redis-cli -h 10.129.94.126 -p 6379 ping
```

Got `PONG`. We're in conversation.

No username. No password. Just vibes.

## Step 4 — What's inside?

```bash
redis-cli -h 10.129.94.126 KEYS "*"
```

`KEYS *` lists everything stored — like doing `ls` in a folder.

Output: `temp`, `flag`, `numb`, `stor`

*They named it* flag. *Not even hiding it.*


## Step 5 - Grab the Flag

Redis has multiple numbered databases — 0, 1, 2... Default is **0**. Connected directly to it:

```bash
redis-cli -h 10.129.94.126 -n 0 KEYS "*"
redis-cli -h 10.129.94.126 -n 0 GET flag
```

Flag. Done.

## What did I actually learn?

Redis is fast, powerful, and completely harmless — *unless someone exposes it publicly with zero authentication.*

No exploit. No payload. Just:
- find the service
- connect directly  
- ask for the flag politely

Also — **always use speed flags for full port scans.** `-p-` alone will have you waiting till next week.
*The database had no lock. I just opened it. Sometimes hacking is just... trying the door.*

— Aarushi, somewhere in the void 🖤
