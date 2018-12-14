---
layout:      post
title:       Moving my DNS records to Route 53 with Terraform
author:      Lucy Davinhart
date:        2017-04-02 17:00:00
category:    tech
tags:        [dns, aws, terraform]
---

I'm one of those people who seems to collect domain names.
I'm even using most of them! But the DNS for these domain names is all over the place. I'm gonna fix that...

The vast majority of my collection I've purchased from GoDaddy.
I'm fine with that. I often hear bad stuff about them, but they've been good to me so I'm in no rush to move.

But the DNS part... that's spread out all over the place.
Some of it is managed by GoDaddy, some of it is delegated to other hosting providers[^1].
lmhd.me specifically is on [PointDNS](https://pointhq.com)[^2].

So I'm going to move it all to one place as much as possible: [AWS Route53](https://aws.amazon.com/route53/)


[^1]: Hostgator, back in my "PHP is cool! Write everything in PHP" days
[^2]: Because that was available as a free add-on for Heroku

### Why Route 53?

There are lots of DNS solutions out there: some free, some paid, some self-hosted. I could have gone with any of them.

But I wanted something I could automate, preferably with [Terraform](https://www.terraform.io/) (because I'm familiar with that from work).

Terraform has [many](https://www.terraform.io/docs/providers/index.html) DNS providers to chose from, and I looked through quite a few of them.

But I eventually decided on Route53, partly because it's cheap[^3], and partly because I'm thinking of moving some of my Heroku stuff over to AWS at some point.

[^3]:
    Route 53 starts at $0.50 per hosted zone per month, which is reasonable enough.
    [Route 53 Pricing](https://aws.amazon.com/route53/pricing/)
    
    Apparently I have so far had 1.24 thousand requests in April (as of 1530 on 2nd April). I can't seem to get historical data though, so I've extrapolated that to ~40k requests per month.
    Even then, it's only $0.52 for lmhd.me per month, according to the [AWS calculator](https://calculator.s3.amazonaws.com/index.html)
    
    PointDNS free limited to one domain, and limited to 10 records per domain. Paid, costs $25/mo, for up to 10 zones. Or $2.50 per zone per month.
    [PointDNS pricing](https://pointhq.com/pricing)
    
    I didn't really compare pricing for any of the others. AWS is cheap enough, and PointDNS isn't.



### `ALIAS` records for my apex domain

The reason I went with PointDNS origiainally was because my apps were on Heroku, and it was available as a free add-on.

The original lmhd.me was a static page on HostGator, then I moved it to Heroku. Now it's on GitHub Pages.

When it was on Heroku, I needed what PointDNS (and others) refer to as an ALIAS record. From the PointDNS console:

> Please specify a DNS name to alias. Point will automatically duplicate A and AAAA records from this address at 15 minute intervals. This may be used as an alternative to a CNAME for the root of a domain.

In the case of lmhd.me this works like:

* `lmhd.me` is an `ALIAS` record for `lucymhdavies.github.io`
* `lucymhdavies.github.io` is a `CNAME` for `github.map.fastly.net`
* `github.map.fastly.net` has `A` records for the relevant IP addresses
* PointDNS creates a `A` records for `lmhd.me` based on the `github.map.fastly.net` `A` records

Route 53 has no such concept. The only Alias records it has is for other AWS services.

I was originally thinking of just setting up `A` records, as [GitHub suggests](https://help.github.com/articles/setting-up-an-apex-domain/#configuring-a-records-with-your-dns-provider), but I can do better than that.

Terraform can itself query DNS, e.g. [CNAME records](https://www.terraform.io/docs/providers/dns/d/dns_cname_record_set.html) and [A records](https://www.terraform.io/docs/providers/dns/d/dns_a_record_set.html).

So what I ended up doing was the following:

```
#
# lmhd.me apex domain --> github pages
#

# Lookup CNAME for lucymhdavies.github.io.
# Result is stored in the variable:
# data.dns_cname_record_set.lucymhdavies-github-io.cname
data "dns_cname_record_set" "lucymhdavies-github-io" {
	host = "lucymhdavies.github.io"
}

# Get IPs for that CNAME
# Result is stored in the array:
# data.dns_a_record_set.github-io.addrs
data "dns_a_record_set" "github-io" {
	host = "${data.dns_cname_record_set.lucymhdavies-github-io.cname}"
}

# Set up the A records for lmhd.me
resource "aws_route53_record" "lmhd-me-A-record" {
	zone_id = "${aws_route53_zone.lmhd-me.zone_id}"
	name    = "lmhd.me"
	type    = "A"
	ttl     = "60"

	records = [ "${data.dns_a_record_set.github-io.addrs}" ]
}
```

This provides me with a good middle ground between simply setting the `A` records manually, and setting up something (a Docker container?) to replicate the functionality PointDNS provides.

### What next?

I'm not done yet. I have more domains to move, but they're all `CNAME` and `A` records, so nothing too interesting with that.
I just need to get around to doing it.

The only manual part of the process is updating GoDaddy's nameservers for the domain to point to Route53.

There's [a Terraform plugin](https://github.com/n3integration/terraform-godaddy) I could use for that, and there are other tools which use the GoDaddy API, for example [a Node.js script](https://www.npmjs.com/package/godaddy-dns)

But given that it's a one-off thing per domain, and I don't have too many domains, I'm not too bothered right now.

Another option for me to consider is that [domains from Amazon](https://d32ze2gidvkk54.cloudfront.net/Amazon_Route_53_Domain_Registration_Pricing_20140731.pdf) cost about the same as I'm paying from GoDaddy.
When that domain is up for renewal in January, I might consider that.

At some point in future I'm going to have Terraform Plan run automatically, and send me a Slack notification if there are any changes to be applied[^4]. But one step at a time, eh?

[^4]: e.g. the A records for GitHub Pages change
