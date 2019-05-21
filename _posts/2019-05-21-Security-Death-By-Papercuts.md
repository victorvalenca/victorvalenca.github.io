---
layout: post
title: "Endpoint Security: Death by a Thousand Virtual Papercuts"
date: 2019-05-21
---

This can be considered a 101 on how not to do endpoint security, where applicable. I have no idea how to properly start this, but a backstory is required:

I recently started off a project that is basically a conglomerate of clients with the sole purpose of migrating their backend from one database engine to another. 

I am part of the "devops" team -- I quote this because the term devops is very loosely defined in this context. I’m responsible for going into the server endpoints via SSH, tinker about with config files, make sure things are okay, run some fabric scripts like a script kiddie, and watch the thing go while sharing my screen over a conference call. We have some pre-migration activities before the actual migration but the process itself goes beyond the scope of this post.

I’m essentially responsible for kicking off the entire migration process, and QA specialists, db admins, etc, come after me to check everything is working out while project management watches and do some text messaging or whatever they can do while we work.

What DOES fall into scope of this post is the absolute hellscape and loopholes we have to go through to even have our laptops able to function in this project.

# Exhibit 1: Slack
TL;DR: Slack is blocked by IT. Through the OS. Accessing ![https://slack.com](https://slack.com) gives an SSL error if we try to open it through work laptops.

This wouldn’t be much of an issue as we don’t really use Slack internally (we use Skype for Business which is MUCH worse). However this project in particular has everyone communicate through Slack, we also got MacBooks to replace our windows laptops (We going full blown Silicon Valley meme startup up in here, no where’s my free starbucks coffee). 

The other teams in the project can use Slack just fine, but for whatever reason my office’s IT department (mind you we are all the same firm, just different offices) decided to block it entirely. We tried connecting from home, from within the office network, from a VPN, from our phones over hotspot, it flat out fails.

Project managers tried talking to IT multiple times, they wouldn’t budge, bringing us to the next exhibit:

## Exhibit 2: Citrix

The workaround for accessing Slack is through a remote desktop instance via Citrix. Thankfully when I joined this project, they had already come up with a solution to the Slack access as a byproduct of having to access other resources through it, but now it brings to our first paper cut: physical RSA device for logging in (which will time you out after about 30 minutes of inactivity). It’s a login consisting of Username + password + passcode from the RSA key. I’ve had to reach for my keys for this too many times and it was getting frustrating. Boy is it frustrating. But hey I can access Slack now! And it’s through a remote Chrome window that has a broken "cleaning browser history" dialog that never goes away. Additionally clipboard and file transfers are completely broken. It ties in very well with the following exhibits, too. 

## Exhibit 3: FortiClient

I can access Slack now, and some of the project resources, but not all of it. That’s through something entirely different, it needs to be done over FortiClient’s VPN. We have a convenient wiki page describing the setup to get our laptops configured to use this program, and we only use it for VPN purposes.

`The FortiClient also functions as an "Internet Security" and "Antivirus" scanner. The "Internet Security" part can be switched off, but the antivirus scanner not.`

Oh boy.

After yolo’ing the installation and getting my poor, fresh out of the box laptop ready for a beatdown, i set up the VPN access and went about my business.
P.S.: Please for the love of god stay away from this client.

## Exhibit 1 Revisited:
One day, coworker from QA team somehow managed to get Slack installed and working on his laptop. I assumed he did some weird shit with his network stack configuration and asked him to go over with me on how he did it sometime after lunch.

He literally just hopped back and forth between networks while loading the Slack pages and it just worked.

What.

It’s persistent and survives system updates and reboots

**What.**

Well, goodbye Citrix, will only need ya if i need to file IT tickets for the project ¯\_(ツ)_/¯


## Exhibit 4: CyOptics and FortiClient VPN
Remember what I said prepping my laptop for a beatdown? Well it happened when CyOptics and FortiClient decided to do their thing. Adding salt to the wound my laptop was warm enough that it wasn’t enough to fire up the fans, but warm enough to make the entire base of the MacBook feel uncomfortable. I started to look at how to kill the AV portion of FortiClient so i only had to deal with one bullshit AV at a time.

Thankfully I found that there IS a stand-alone VPN client so I yeeted the sucker out of my hard drive and installed that less cancerous program.

And lastly:

![](/img/panic_at_the_kernel.png)
￼
I’ll let that one speak for itself.
