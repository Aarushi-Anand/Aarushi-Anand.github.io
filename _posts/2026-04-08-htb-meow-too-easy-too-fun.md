---
title: "HTB Meow — Yeah, I Hacked a Cat."
date: 2026-04-08
categories: [HTB, Writeup]
tags: [htb, ctf, telnet, beginner, nmap]
---

Okay so. First machine. Meow. Cute name. Dangerous reality. Let's go.

## What even is this?

HTB Starting Point, Tier 0. They basically hand you a machine and say
"figure it out." No hints. No hand-holding. Just you, a terminal, and
your questionable life choices.

## Step 1 — Are you even there?

Connected to HTB VPN, spawned the machine, grabbed the IP. First thing?
Ping it. Because trust issues.

```bash
ping <target_ip>
```

It replied. Good. We're talking.

## Step 2 — Knock knock, nmap

```bash
nmap -sCV <target_ip>
```

One port open. Port 23. Telnet.

TELNET. In 2026. On a live machine. I felt secondhand embarrassment
for whoever set this up.

## Step 3 — Walk right in

Telnet is that one protocol that never got the memo that passwords exist.

```bash
telnet <target_ip>
```

It asked for a username. I typed `root`. No password.

IT. JUST. LET. ME. IN.

No drama. No fight. Just — welcome, here's your root shell, make
yourself at home. 

## Step 4 — Flag goes brr

```bash
ls
cat flag.txt
```

Flag submitted. Machine pwned. 

## What did I actually learn?

Telnet = legacy = danger. If you ever see port 23 open on a real
system during a pentest, that's not a finding — that's a gift.
Default credentials and no authentication? That's how whole networks
fall.

Meow was easy. But easy machines teach real lessons. 

— Aarushi, somewhere in the void 🖤
