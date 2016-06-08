# Jekyll-Mono

**Jekyll-Mono** is a simple and elegant GitHub Profile cum Blog theme based on Barry Clark's [Jekyll-Now](https://github.com/barryclark/jekyll-now). It's a result of my attempt to learn **Jekyll** and create a minimalistic theme to put up my CV alongwith some blog posts.

It is crafted with 💙 by [Akshay Agarwal](https://github.com/AkshayAgarwal007).

![Jekyll-Mono Home Page](/images/ss.png)


## What is Jekyll?

It is a static site generator. It takes your content written in Markdown, passes it through your templates and spits it out as a complete static website, ready to be served using GitHub pages for free.
Because your entire blog is static it serves and perform faster. It consumes less web resources namely memory and I/O.

## Getting Started

Let's quickly set up your new blog in a matter of minutes.

### Fork this repository

Hit the “Fork” button in the top-right corner of the repository to fork a copy of this theme to your GitHub account and rename it to **yourusername.github.io** and then visit https://yourusername.github.io and you'll be able to see your newly created blog using Jekyll-Mono.

### Customise Jekyll-Mono

So now your blog is live with its default settings. Let's customise it now.

Edit the _config.yml and enter your site name and description. You can easily turn on Google Analytics tracking, Disqus commenting and cool loking social icons here too.

Jekyll-Mono also comes with the option of setting up the color scheme of your blog. You can do this by editing _variables.scss that lies inside the _sass folder. You can change the main theme color by simply replacing the current hex color value of `$mono` with the one of your choice. A few main theme sample colors are included in there as comments. Apart from the main theme color you can also change the header link color, navbar hover color, hyperlink color as well as the color of the various headings.

A look at the customisations you can do with _variable.scss

```
// Main theme colors 
// Some cool main theme colors(violet:#8476ad;blue:#5cacee;red:#ff7373,#ff6f69;green:#6acf64,#2ddbb3;orange:#ffa268)

$mono-color:#8476ad;                // main theme color(header, links, footer icons, buttons, post-title)
$hl-color: $darkGray;              // header link color (author name and posted on date) for blog post meta 
$navbar-hover-color:$gray;        // navbar hover color (site name and navbar links hover color)
$link-color: $darkerGray;        // normal hyperlink color other than the ones above.


// Heading colors
// You can play around with these too!
$h1-color: $mono-color;
$h2-color: $mono-color; 
$h3-color: $darkerGray; 
$h4-color: $gray;
``` 
Have a look at Jekyll-Mono with four different main theme colors.
 
![Jekyll Mono in 4 different colors](/images/ss-color.png)

Finally you need to set up your avatar. Pick up your avatar, resize it to 220x220px simply using paint or any editor of your choice and upload it to the images folder. Now open _variables.scss, you'll see something like this `$avatar: "/images/avatar.jpg";`. Here change the avatar.jpg to what you have uploaded just now.


### Start Blogging

Publish your first blog post by editing /_posts/2016-03-06-Eternal-Lorem-Ipsum.md. [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) might come in handy while writing your blog posts in Markdown. If you are not comfortable with writing in Markdown you can use [Prose](http://prose.io/) for writing your blog posts. [This](https://developmentseed.org/blog/2012/june/25/prose-a-content-editor-for-github/s) will help you in setting up prose.

![Post](/images/post.PNG)

To create a post just click on create new file button in /_posts/. Create a new file with the following naming convention: **year-month-day-title.md**. Also make sure to include the front-matter at the top of each new blog post.

You can see the front matter at the top of /_posts/2016-03-06-Eternal-Lorem-Ipsum.md. It looks something like this. You have to change the title and author according to your post. The layout will remain the same.

```
--- 
layout: post
title: The Eternal Lorem Ipsum Placeholder Text Here
author: Author Name
---

```

### About and Projects Page

In the parent folder you'll find about.md and projects.md. These are templates for helping you to set up your biodata/CV and projects that will be available at yourusername.github.io/about and yourusername.github.io/projects respectively. Again you can edit it in Markdown on GitHub or using Prose as per your choice.


## Demo

You can see the live demo of this theme at http://akshayagarwal007.github.io/Jekyll-Mono/

## Local Development

For setting up your development environment you can follow the official Jekyll [Documentation](https://jekyllrb.com/docs/installation/). It's a little bit complicated for Windows users. Let's see how it's done on windows:

1. **Install Chocolatey**. **[Chocolatey](https://chocolatey.org)** is a package manager for windows. It's awesome. 
   Open a command propmt with administrator access. Paste this. Hit Enter and restart the command prompt.
   
   ```
   @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
   
   ```

2. **Install Ruby** : In your command prompt type `choco install ruby -y`.
 Hit Enter. After installation restart the cmd with administrator privileges.

3. **Install Jekyll/Sass (plug-ins used by GitHub Pages)**: `gem install github-pages`

4. **Clone your fork of Jekyll-Mono** `git clone https://github.com/yourusername/yourusername.github.io.git`

5. **Serve the site** by `jekyll serve`. It regenerates itself after any changes has been made.

6. **View your site** at http://127.0.0.1:4000/

7. If you make any changes to _config.yml you'll need to rebuild the site using `jekyll build` and then again a `jekyll serve` to serve it. You'll need to do a rebuild only if you want to see the changes locally, else you can simply commit your changes and push them to your GitHub repo, GitHub pages will rebuild and serve your website.

## Credits

* [Barry Clark](https://github.com/barryclark) for creating [Jekyll-Now](https://github.com/barryclark/jekyll-now) on which this theme is based.
* [Manoela Ilic](https://github.com/crnacura) of Codrops for writing awesome articles on web design.
* [David Miller](https://github.com/davidtmiller) of Blackrock Digital for making awesome open source themes.
* [Jekyll](https://github.com/jekyll/jekyll) - Thanks to its creator and contributors.
* [Font-Awesome](http://fontawesome.io) - Thanks to its creator and contributors.

## Contributing

* Found a bug? Report it on GitHub [Issues](https://github.com/AkshayAgarwal007/Jekyll-Mono/issues) and include a code sample.
* [Fork](https://github.com/AkshayAgarwal007/Jekyll-Mono/fork) the repository and start your own blog using it and let me know about it so that I can include a reference to it here. Probably the best way you can contribute :)
* If you find anything that's wrong or want to talk to me about anything related to this theme or want to contribute in any way, please feel free to [mail me](mailto:agarwal.akshay.akshay8@gmail.com).

## License

Jekyll-Mono is licensed under [MIT](https://github.com/AkshayAgarwal007/Jekyll-Mono/blob/master/LICENSE.txt).










  
