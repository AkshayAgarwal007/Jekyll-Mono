---
layout:      post
title:       Moving all my DNS stuff under one roof
description: 
author:      Lucy
date:        1970-01-01 00:00:00
category:    tech
---

e.g. Route 53
I have most of my vast domain name collection purchased by GoDaddy.
I'm fine with that.
I often here bad stuff about them, but they've been good to me.

But the DNS part... that's spread out all over the place.
Some of it is managed by GoDaddy, some of it is delegated to other hosting providers.
I'm gonna move that all (as much as is possible) under one roof: probably Route 53, because it's AWS, and because it's fairly cheep.
Maybe. I'll see what else works.

I Googled "dns provider" and found this
https://www.keycdn.com/blog/best-free-dns-hosting-providers/
But I should probably check other places too.

CloudFlare might be a good one.
That has SSL apparently.

Other option, just stay with GoDaddy and automate that
https://www.npmjs.com/package/godaddy-dns
https://github.com/n3integration/terraform-godaddy
Then delegate subdomains to hostgator as necessary
https://networkhobo.com/2014/07/22/godaddy-delegate-subdomain-to-different-nameserver/
