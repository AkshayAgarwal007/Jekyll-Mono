---
layout: post
title: Testing Netlify CMS
description: Netlify CMS seems pretty cool. How well does it work?
author: Lucy Davinhart
date: 2018-12-14T15:14:42.423Z
category: tech
tags:
  - test
  - netlify
  - netlify cms
---
While LMHD.me is primarily a landing page with links to my social, I do occasionally post long-form posts to it. I don't have a link to them on the front page (yet), but I might do that again at some point.

I've added [Netlify CMS](https://www.netlifycms.org/) to my site, because it's pretty cool, and seems like a good idea.

The instructions I followed are here: <https://www.netlifycms.org/docs/add-to-your-site/>

I also used an example config file from here: <https://github.com/netlify-templates/jekyll-netlify-cms/blob/master/admin/config.yml>

In the case of my blog, that was two commits:

<https://github.com/lucymhdavies/lucymhdavies.github.io/commit/2755181bb96d1b19c129257fe513874e802bc82f>

This is adds the admin page, Netlify CMS config, and scripts on the main page to redirect me to the CMS once I log in.

I also needed this, because copypaste errors are _the best_

<https://github.com/lucymhdavies/lucymhdavies.github.io/commit/bd48bb649f76c2b2423162873ee669c687a504f4>

Does it work?

Yes.

Yes it does. And it's really nice.

I log into it using Netlify Identity service, using my Google account. I can create a new post, and when I save it it immediately creates a new PR in my GitHub Repo:

<https://github.com/lucymhdavies/lucymhdavies.github.io/pull/5>

This in turn triggers an automatic deployment, so I can preview my changes.

The only slight annoyance I have with it so far is that my current posts and images are structures like so:

```
🐳 :lucymhdavies.github.io lucy $ tree _posts/
_posts/
├── 2017
│   ├── 03
│   │   ├── 2017-03-30-Init.md
│   │   └── 2017-03-30-jekyll-from-ios.md
│   └── 04
│       └── 2017-04-02-dns-under-one-roof.md
├── 2018
│   ├── 03
│   │   └── 2018-03-15-nail-polish.md
│   └── 07
│       └── 2018-07-22-emoji-graphs.md
└── cms

7 directories, 5 files

🐳 :lucymhdavies.github.io lucy $ tree images/
images/
├── 404.jpg
├── LMHD_xs.png
├── avatar.jpg
├── bg.jpg
├── both.gif
├── cms
├── post.PNG
├── posts
│   ├── 2018-03-15
│   │   └── captain-obvious.jpg
│   └── 2018-07-22
│       ├── attention.gif
│       ├── emojitracker.png
│       └── final-graph.png
├── ss-color.png
└── ss.png

4 directories, 12 files
```

And Netlify CMS doesn't appear to have an obvious way of nested subdirectories for images and posts, meaning it doesn't automatically see my previous posts.

No matter. That's easily fixed. I moved my posts to a common directory:

<https://github.com/lucymhdavies/lucymhdavies.github.io/commit/2047f087a2985a1f732dc1bd45bdd81817a8b7d0>

With that change, Netlify CMS picked up my existing posts

![Screenshot of CMS, with 5 posts listed](/images/cms/screen-shot-2018-12-14-at-15.49.19.png)

I've not done this for images yet, as I'd need to go through all my existing posts and update them inline too. I might do that at some point, but I'm not too worried for now.

But all-in-all... Seems pretty cool so far :)
