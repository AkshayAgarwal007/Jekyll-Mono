---
layout: post
title: How to Jekyll from iOS
description: This post edited entirely on my iPad
author: Lucy
date: {}
category: blogging
published: true
---
Most of the editing I do on this blog is/will be done on my laptop, and mostly in Vim. But that is not really great if I want to post something while I am away from my laptop.

So I need to figure something out.

## Prose.io - Free

http://prose.io/ seems good. I think it is designed specifically for Jekyll sites. I have been writing this post in that site so far, and it works okay, but not great.

Most annoying is that it does not take advantage of iOS's autocorrect[^1], e.g. spelling, uppercaseing first letters, double-tap space for full stop.

Plus it's a web app, so I need Internet access to use it.

But it does integrate directly with GitHub, so I save posts and drafts easily.

So while this does kinda work, it's not great, and I can see myself getting frustrated with it quite easily.

So. Let's find other options.

## Octopage - £0.99

https://appsto.re/gb/rk9UM.i

This app actually works quite well. It has offline editing, preview, and a Markdown syntax guide.

There are a few bugs though, the most annoying of which is that it occasionally scrolls up when I preview the post then go back to edit mode.

It also only has drafts local to the app. No way (yet) of using the drafts directory in the repo.
I've asked the dev to add that functionality, so we'll see what happens.

That's not a deal breaker, but it does mean that any post I don't complete in this app can't be picked up on my laptop.

Which means that, while I can write my thoughts on the app using the app itself, I need to update my page draft with a different app.

## Source for iOS - Free?

https://appsto.re/gb/r06Sgb.i

App is actually free, but being able to push back to GitHub is an in-app purchase.

This one took a while to get started. It wasn't able to set up my SSH key, so I had to add that manually. Once that was in place, it all went swimmingly.

This one is a full fledged git client, so it lets me edit any file in the repo. And it also has offline editing, which is nice.

No preview functionality, but that's fine; that's not what the app is for.

It does have a few keyboard issues though. It has its own keyboard, which can be used in other apps. It looks like it is supposed to have quick access to special characters, but I can't see how that is supposed to work. You can disable the custom keyboard, but even then there is none of iOS's autocorrection, which makes it frustrating to use.

I would like to love this app, but I would need more keyboard behaviour settings before I am happy with it.

I have listed it as free with a question mark becauase it says that pushing to a remote repo is part of the £4.99 in app purchase, but it looks like that just works in the free version. I suspect the answer is that pushing to other remote repos is the premium feature. Or it could be a bug, because when I try to push without committing at the same time, it prompts me to pay for then in app purchase.

Note to self: contact the dev about keyboard preferences.

## So... what will I use?

TBC

[^1]: That alone is interesting to me. I want to see how this app works, because it is obviously doing something odd.
