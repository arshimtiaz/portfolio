---
title: "My First Wi-Fi Pentest"
date: 2025-07-30
description: "How I fumbled my way through capturing and cracking my first Wi-Fi handshake using Aircrack-ng."
author: "Arsh Imtiaz"
tags: ["wifi", "aircrack-ng", "pentesting"]
---

![Wi-Fi Pentesting](/images/wifi-pent.png)
# My First Wi-Fi Pentest

There’s a massive difference between watching pentesting videos and actually doing it. This was my first time seriously trying Wi-Fi pentesting, and it was a mix of pure excitement, a bunch of silly mistakes, and that one moment of “YES! It finally worked.”

**Disclaimer: This is just my story, not a tutorial. Everything I did was on my own network. Don’t go around trying this on random Wi-Fi unless you like the idea of explaining yourself to law enforcement.**

---

## Gearing Up
My setup was simple – my Arch Linux laptop, an Alfa adapter, and the Aircrack-ng suite.  

<div style="text-align: center;">
  <img src="/images/adapter.png" alt="Alfa AWUS036ACS" width="60%">
  <br>
  <small>My trusty Alfa adapter, aka "the antenna that could"</small>
</div>

At this point, I was overconfident. In my head, it was just: “Enable monitor mode, grab handshake, crack password. Done.”  

> Spoiler: life doesn’t work that way.

---

## Where I Fell Flat
The very first thing I did? I forgot to disable all the services that fight you for control of the adapter.  
Result: Warnings everywhere, services kept messing with monitor mode.

<div style="text-align: center;">
  <img src="/images/airmon-ng.png" alt="warnings everywhere" width="100%">
  <br>
  <small>When your OS says “nah”</small>
</div>

Once I figured that out, things finally started looking cool. My terminal was scrolling with access points like some cyberpunk movie scene.

<div style="text-align: center;">
  <img src="/images/airodump-ng.png" alt="warnings everywhere" width="100%">
  <br>
  <small>Access point recon (cool hackery stuff)</small>
</div>

---

## The "Why Is Nothing Happening?" Phase
I locked onto my target, sat back, and… nothing. No handshake.  

Turns out, distance *does* matter. I was sitting too far from the router like some wannabe hacker in a corner. Moving closer fixed everything, and that glorious “Handshake captured!” message finally appeared.

---

## The Brutal Reality Check
Cracking it? Yeah, that part didn’t happen. My wordlist didn’t have the password (because of course it didn’t). Turns out a [14 million password wordlist](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt) doesn't contain the password from all over the universe. (because of course it doesn't).

But honestly, just getting that handshake felt like a small win. I’d done the whole process myself and figured out where I’d gone wrong.

---

## Lessons Learned
- The tools are just tools. Knowing how to troubleshoot when things fail is the real skill.  
- Proximity is underrated. Sitting 10 feet further might be the difference between success and nothing happening.  
- Wordlists are everything. A bad one means hours of wasted time.
- There are certainly *other methods* of "getting passwords" from your target other than using wordlists \*wink\* (will cover this in the future)

---

## Why I’m Glad I Did It Manually
I could’ve just used [**Airgeddon**](https://github.com/v1s1t0r1sh3r3/airgeddon) or some automated script, but figuring it out command by command taught me *why* things work, not just how to click buttons.  

Next up? I’ll see if I can get Airgeddon to make my life easier *without* making me lazy.

---

<div style="text-align: center;">
  <img src="/images/hackerman.gif" alt="Hackerman">
</div>


This wasn’t some cinematic “Hollywood hacking” moment. It was messy, frustrating, and way less glamorous than it looks online. But that’s what made it fun.  

The real flex isn’t showing commands – it’s showing that you understand **what’s happening under the hood** and can figure things out when they go wrong.
