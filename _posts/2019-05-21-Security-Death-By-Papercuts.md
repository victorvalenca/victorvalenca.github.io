---
layout: post
title: "Endpoint Security: Death by a Thousand Virtual Papercuts"
date: 2019-05-21
---

This can be considered a 101 on how not to do endpoint security, where applicable. I have no idea how to properly start this, but a backstory is required:

I recently started off a project that is basically a conglomerate of clients with the sole purpose of migrating their backend from one database engine to another. 

I am part of the "devops" team -- I quote this because the term devops is very loosely defined in this context. I’m responsible for going into the server endpoints via SSH, tinker about with config files, make sure things are okay, run some fabric scripts like a script kiddie, and watch the thing go while sharing my screen over a conference call. We have some pre-migration activities before the actual migration but the process itself is irrelevant.

I’m essentially responsible for kicking off the entire migration process, then the QA specialists, db admins, etc would follow up after me to check everything is working out while project management watches and do some text messaging or whatever they can do while we work.

This is a little beyond scope of this post, but what DOES fall into scope is the absolute hellscape and loopholes we have to go through to even have our laptops able to function in this project.

## Exhibit 1: Slack
TL;DR: Slack is blocked by IT. Through the OS. Accessing [https://slack.com](https://slack.com) gives an SSL error if we try to open it through work laptops.

This wouldn’t be much of an issue as we don’t really use Slack internally (we instead use Skype for Business which is MUCH worse). However this project in particular has all teams communicate through Slack, we also got MacBooks to replace our windows laptops (We going full blown Silicon Valley meme startup up in here, now where’s my free starbucks coffee?). 

The other teams in the project can use Slack just fine, but for whatever reason my office’s IT department (mind you we are all the same firm, just different offices) decided to block it entirely without question. We tried connecting from home, from within the office network, from a VPN, from our phones over hotspot, it flat out fails. Nope, not having it, IT doesn't want us to use it because something something TLS 1.0 and 1.1 (literally what the fuck, guys)

Project managers tried convinving IT multiple times, they wouldn’t budge, bringing us to the next exhibit:

## Exhibit 2: Citrix

The workaround for accessing Slack is through a remote desktop instance via Citrix. Thankfully when I joined this project, they had already come up with a solution to the Slack issue, mainly a byproduct of our need to access other resources through Citrix. This brings us to our next paper cut: physical RSA device for logging in (which will time you out after about 30 minutes of inactivity). It’s a login consisting of Username + password + passcode from the RSA key, and it's paaaaainfully laggy. I’ve had to reach for my keys for this too many times and it was getting frustrating. Boy is it frustrating. But hey I can access Slack now! 

Through a remote Chrome window!

With a broken "cleaning browser history" dialog that never goes away!

Oh yeah and clipboard + file transfers are completely broken. It's an absolute joy to use. 

## Exhibit 3: FortiClient

I can access Slack now, and some of the project resources, but not all of it. That’s through something entirely different, it needs to be done over FortiClient’s VPN. We have a convenient wiki page describing the setup to get our laptops configured to use this program, and we only use it for VPN purposes.

`The FortiClient also functions as an "Internet Security" and "Antivirus" scanner. The "Internet Security" part can be switched off, but the antivirus scanner not.`

Oh boy.

After yolo’ing the installation and getting my poor, fresh out of the box laptop ready for a beatdown, I set up the VPN access and went about my business.

*P.S.: Please for the love of god stay away from this client.*

## Exhibit 1 Revisited:
One day, coworker from QA team somehow managed to get Slack installed and working on his laptop. I assumed he did some weird shit with his network stack configuration and asked him to go over with me on how he did it sometime after lunch.

He literally just hopped back and forth between networks while loading the Slack pages and it just worked.

What.

It’s persistent and survives system updates and reboots

**What.**

Well, goodbye Citrix, will only need ya if i need to file IT tickets for the project ¯\_(ツ)_/¯


## Exhibit 4: CyOptics and FortiClient VPN
Remember what I said prepping my laptop for a beatdown? Well it happened when CyOptics and FortiClient decided to do their thing. Both processes firing up and battling it out for CPU time. 

Trivia: The few results I found for CyOptics was [this paper about how it uses AI](https://www.cylance.com/content/dam/cylance/pdfs/data_sheets/CylanceOPTICS.pdf) (read: if statements) to detect threats.

Speaking of Cylance, [here's a fun read that goes beyond the topic of this post](https://arstechnica.com/information-technology/2017/04/the-mystery-of-the-malware-that-wasnt/)


Adding salt to the wound my laptop was getting warm... Too warm... but not enough to fire up the fans. So I have this slab of alluminium to type on that is extremely uncomfortable and the CPU temperature just teeters close to the "max out the fans" threshold. It didn't take long for me to start looking at how to kill the AV portion of FortiClient so I only had to deal with one bullshit antivirus at a time.

Thankfully I found that there IS a stand-alone VPN client so I yeeted the sucker out of my hard drive and installed that less cancerous program.

And lastly:

![](/img/panic_at_the_kernel.png)

I’ll let that one speak for itself.
