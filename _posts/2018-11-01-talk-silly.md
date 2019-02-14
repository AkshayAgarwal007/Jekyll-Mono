---
layout: post
title: Talk - Making Silly Things
description: Slides and Speaker Notes for my GoSheffield talk on making silly things
author: Lucy Davinhart
date: 2018-11-01T18:00:00.000Z
category: tech
tags:
  - grafana
  - prometheus
  - emojis
  - impostor syndrome
---

Presented at [GoSheffield, November 2018](https://www.meetup.com/GoSheffield/events/255018075/)


# Slides

[![Title Slide. Upside down smily face emoji, @LucyDavinhart, Making Silly Things](/images/cms/silly-talk-title-slide.png)](https://talks.lmhd.me/silly.html)

[https://talks.lmhd.me/silly.html](https://talks.lmhd.me/silly.html)

# Script / Speaker Notes

[D] 🙃 (Why you should build silly things)

## 0. Title slide [30s]
[30s]

`<show emoji and twitter handle>`

As much as I’d like my talk to be named Upside Down Smiley Face Emoji, you’d have no idea what to expect seeing that on meetup.com

`<reveal title>`

So I’m gonna talk about making silly things, and why you should do it.

## 1 Emojis - Example of being Silly [4.5m]
[5m]

Let’s start with an example. I have a reputation for perhaps being a little enthusiastic when it comes to emojis. And so while ago, this manifested as this:

`<screenshot of emoji grafana dashboard>`
- This is a screenshot of something I put together one Friday afternoon at work.

`<live view>`
- And here it is live. 
- What you’re looking at is a graph of emoji usage on Twitter, in close to real-time. I’ve filtered it to one unpopular one in particular. If the demo gods will allow, here’s a tweet that proves it. 
- So this is built on top of something called emojitracker, which uses the twitter apis to keep track of all tweets containing emojis, in real-time. And it has its own streaming api, which returns a stream of linebreak-separated-json with stats about what emojis have been used recently. 
- The part I wrote takes that data and outputs a Prometheus metrics page, which I’m showing here as Grafana graph. So this graph is 3 or 4 APIs abstracted from what’s going on on twitter. 
- If you’re unfamiliar with Prometheus, it’s a really good tool for monitoring your systems. Usually used for things like how much CPU idle a particular server has, or memory usage, disk space. It has a lot of integration, but you can write your own custom integrations with their go SDKs, which is what this is.

And you may be asking yourself... why?
- And I made this for two reasons:

Firstly, Just Because.

This silly idea of mine was prompted by a twitter bot I found which was tracking the least used emojis in emojitracker, tweeting it out on a schedule.
- The only thing even resembling a purpose for it was as part of an experiment, to test a theory I had, to see if I could predict what people would do in response to this bot. I suspected that, with this bot getting attention, that would be enough that what was the least used at the time would eventually become the second least used.

Crossover point
- And that allowed me to get this graph, which is the moment that happened, about 2 days after I created this, about 12 hours later than I predicted based on linear regression. 
- And it didn’t actually take that long to put together either. I said an afternoon, but it’s building on a similar thing I put together a few weeks prior which was getting stats from twitch.tv, so I’d already done the hard work. I’m not gonna show the code on screen, because it’s not that interesting, and not actually very good. But you can dig it up from my GitHub later if you want. 
- But both this, and the original Twitch version, gave me a much better understanding of how Prometheus exporters work, which meant that a few weeks later, when I needed to do one for my job, it wasn’t a strange new concept that I’d have to learn.




## 2 - So why am I talking about this? [1.5m]

First we have to go back a few years…


## 3 - Rewind

- Back when I was about yay tall, went by a different name, and thought Visual Basic 6 was the coolest thing ever, I’d make all sorts of random stuff for fun. I didn’t really know what I was doing, and was blissfully ignorant of that fact; I was just making stuff, because I had ideas in my head I needed to get down on silicon. 
- What happened? 
- It wasn’t until I got to university that I was actually properly taught how to write code. There was suddenly this idea that there was a right or a wrong way of doing things, and that my code could be bad. It didn’t help that I was surrounded by smart people who were better than me (my brain conveniently filtered out the people who weren’t). 
- My skills improved, and my code got much better, as I learned better ways of doing things, but I also got to see and appreciate what quality looked like… and there was a gap between that and my work. I wasn’t there yet, so I’d feel like a failure.



## 4 - Impostor Syndrome - The problem [3.5m]

Impostor syndrome begins

- This is impostor syndrome, a feeling that you don’t actually know what you’re doing, despite evidence of the contrary. Apparently about 70% of people get it at some point in their lives. 
- It got worse for me when I started my first job, so in addition to being surrounded my smart people, I was surrounded by people who understood the massive million-lines-of-code monoliths the company developed. Let’s ignore the fact that no one person really understood the whole thing. 
- It was exhausting, and the side projects I was working on at home stalled.


Fear of harsh judgement
- So part of this comes from a fear of being judged harshly. That people will look at your work and mock it for being so bad.

Fear of harsh judgement, cont.
- This fear of judgement in my case is not just limited to coding, but is sort of a horizontal slice through all aspects of my life. 
- For example, I’m a woman, and a trans woman at that, I’d find myself sometimes not fitting in with the heavily male dominated industry. I tried to fit in, so that I wouldn’t be judged for being different, but I ended up spending energy on that which could have been better spent elsewhere. 
- `<wonder women who go> <trans gopher>`


Fear of Failure
- This is something I struggled with for the longest time; when faced with a new technology I need to use for my day job, I find it hard to admit that learning something new under time pressure can be stressful, and take longer than anticipated. 
- This often leads to me staying late in office if I don’t make as much progress as I had hoped, which is something I’m working on getting better at. 
- Something I realised was happening with with my side projects was that I wasn’t working on things because I expected them to fail. I’d cling on to ideas as “something I’d work on when I’m better at this”, because I didn’t want them to fail. But of course, that meant I wasn’t making them.


Big Ideas are overwhelming
- There’s this idea I first learned about in the context of art, which is that as you become a better artist, you get better at appreciating quality faster than your ability to create improves. So you feel like you’re getting worse, when what’s actually happening is your goals are getting bigger. 
- That’s something which definitely happened to me; I was that kid in uni who thought they were gonna create the next big social network that everyone would want to use… based around an idea which, in retrospect, is actually maybe only useful for me and a few other people, and which could probably be achieved with something much simpler than what I had in mind. But I didn’t know that back then, so I’d accumulate feature creep over time without so much as writing one line of code. 




## 5 - The Solution? [5.5m]

These days, I have the word “Senior” in my job title, so people seem to think I’m awesome. They might be right... [pause for laughter?] but nah, the impostor syndrome is still there. I’m just getting better at dealing with it.



Doing Stuff Because It Scares You
- So this goes back to fear of being judged. Turns out, asking people to review my code… not that big a deal. Everywhere I’ve worked so far, any time I submit things for review, I get constructive feedback. Yes there are often problems with the code, but it’s clear that those are not problems with me, and I always learn how to do things better in future. 
- I’ve taught people how stuff works, both at my old job and my current job. That was a weird feeling at first: impostor syndrome telling me I didn’t know what I was talking about, and me proving it wrong by explaining stuff to people. 
- And of course, I’m standing in front of you giving this talk, despite the fact that just last night I was convinced I had no idea what I was talking about. 
- Doing scary things makes doing scary things easier. 
- And over time I’ve learned to be myself more, and I’ve found myself not worrying so much about what others think of me.


Prototypes & Silly Things

- Big ideas can be great, but they’re made up of lots of small bits, and unless you’re a super genius you won’t immediately know how to make the entire thing in one go. So biting off small bits, experimenting with ideas related to it, and building up your knowledge, until eventually you do have all the skills you need to build something big if you want to.

- This should be obvious, but it’s something I used to undervalue. 
- Experiment. Prototype stuff. Try lots of things. If one works out well, great, you can improve on it. 
- Often when you have an idea, you have no idea if it’s any good, so you can spend hours or weeks or months over engineering the most elegant of things... only to find that actually, it wasn’t that great an idea in the first place. 
- I have a git repo full of unfinished experiments, where I wanted to try out a new library or an idea I had. 
- Some of those ideas have graduated to their own repos, like the emoji exporter. Some are still little things I’m tinkering with every now and then. None of them are examples of my best code; all are ideas I wanted to try out. 
- In the spirit of learning not to be afraid of writing bad code, I keep it open. I don’t imagine many people are going to find it, but it’s there should I ever want to point somebody at a thing I’ve been experimenting with.



L&D Time

- You may remember I said this emoji exporter was done on a Friday afternoon at work. 
- I wasn’t slacking off, honest; we have a thing called Learning & Development time where we can work on stuff that’s not work related, or read a book, watch training videos, etc. Anything to further our own learning and development. 
- This is something I’ve seen a few companies do. For me personally, I get silly ideas like that Prometheus exporter for twitter emojis all the time, and before L&D time was a thing, I’d make a note of them, but never really find time outside of work to focus on them. 
- And one of the great things about this is that it teaches you to give yourself Permission to Fail. 
- L&D time is for you to do what you want, you’re not committing to shipping anything at the end of it. With this structured L&D time, or even in your own time making something silly without any commitment to producing anything of value at the end of it, you’re training yourself to try new ideas, and accept that sometimes stuff doesn’t work out. Then in theory, when you’re struggling with your day job, it becomes less difficult to admit that it’s not working out and that you might need help. I’m still working on that one, but this sort of thing really helps with that.

Go! [2.5m]

- And I want to bring things back to Go; this is Go Sheffield after all. 
- While Ruby was the language that got me back into working in side-projects, it was Go that made things fun again. 
- There are several thing which really drew me to Go which I want to highlight 
- Superficially, it’s impossible to write badly formatted Go code when GoFmt and GoImports exists, and can be configured to run automatically when you save. I know this isn’t unique to go. Saves me from being distracted, and just get on with writing the code. 
- It’s also really easy to read and understand, especially when it come to figuring out what comes from which libraries. This sort of thing makes it much easier to teach yourself the language than, say, Ruby, where I’ve often seen package names that don’t quite match the name of the gem which provides them, and monkeypatched functions that you just have to know where things are coming from. 
- But I think mostly, the reason is that when I first started learning go, like when I was learning ruby, and like when I was learning Visual Basic 6 back in the day... I was learning by myself how it worked rather than being taught. And it seemed super intimidating at first (my first Go project was this tool that someone in my team wrote that was complicated and intimidated me. He’s now gone off and is doing other things, and now I’m the expert in this particular tool). 
- I can look back and see how far I’ve come with it, so when I look forward at all the things i know exist but which I’ve not used before, i know that I can teach myself how to do anything I want with it. 
- And that even if I don’t have an obvious use for some features now, that I’ll be able to come up with something silly I can use to teach myself how it works.


## 6 - End

So yeah, that’s all I have for you.

Look for things you don’t understand, and come up with silly ideas as an excuse to experiment with them.

Thank you!
