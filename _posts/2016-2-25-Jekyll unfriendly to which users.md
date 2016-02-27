---
layout: post
title: Jekyll&#58; friendly to 99 out of 100 users
tags:
- jekyll
- meta
---

<img src="/images/screenshots/jekyll-logo.png" class="blogpost-right-small" />My old blog ran on Wordpress, which was a reliable source of annoyances.  Formatting annoyances, mostly.  Links were nudged above or below the rest of the 
text.  Post previews mysteriously blanked 
out.  Themes didn't work well on mobile devices.  After yet another evening wasted 
combing through other people's CSS, I was ready to change platforms – but to what?

A friend suggested [Jekyll]( http://jekyllrb.com/docs/installation/).  It's free, open-source software, and you can easily tweak any of the code that produces the actual HTML, so it gives you a lot of control over your page 
layout.  I particularly appreciated how easy Jekyll 
made it to use [Bootstrap]( http://getbootstrap.com/), a platform-independent 
framework for creating webpages that automatically reconfigure themselves 
for phones, tablets, or old-fashioned PCs.  More and more, I see people using mobile devices as their primary browser; it's bad enough for a website to be ugly, but making it unusable for half the people who'd like to see it 
is even worse.

As with so many open-source projects, the documentation for Jekyll was not all that great (though it seems to have improved as recently as the past few months).  I learned the ropes by downloading the sample code, reading through it, and figuring out which files Jekyll used to generate the different parts of a page.

The post you're reading right now, for example, uses the `default` template to generate the page headers, and the `post` template for the HTML that all the blog posts have in 
common (like the title and the date stamp).  Some elements, like the Google Analytics web tracking code, come from a configuration file.

The templates use the [Liquid]( http://liquidmarkup.org/) library, which is deliberately limited to keep it from introducing security holes.  Oh, and the actual blog posts themselves are written in a markup language called [Markdown]( https://daringfireball.net/projects/markdown/syntax#code), which takes a text file and converts simple notation into HTML, except when it 
doesn't.  (Mostly when there's more Bootstrap code than Markdown cares for in the file.)

Once you have everything up and running, Jekyll automatically creates all the ncessary webpages as HTML files.  Wordpress 
and similar platforms produce pages *dynamically*, usiing a program to create them when a user requests 
them, but Jekyll creates *static* pages: it generates the HTML once and stores it in a file.  If you change anything – by editing a page, say, or adding a blog post – Jekyll automatically regenerates any pages that need to be changed.  (So when 
you add, say, a blog post, Jekyll will create the post itself, update the blog 
pages which list all the posts in order, and update any relevant tag pages.)

Jekyll 3 requires [Ruby v2 or later]( https://www.ruby-lang.org/en/downloads/), which unfortunately means I can't run it on the servers at Bluehost, 
where my domain was already hosted.  And I didn't want to shop around for a new host when I still had plenty of paid-up hosting time.  So I just use my own computer as a test bed, making sure I'm happy with what Jekyll generates before 
running a Ruby script I wrote that pushes everything up to the Bluehost servers.  (Testing offline is probably a better method anyway - the main gotcha is that
the push script can potentially introduce bugs when, say, a Jekyll plugin changes
the directory structure a little too much from what it's expecting.)

It takes a little longer to get up to speed with Jekyll than it does with Wordpress.  But there's another trade-off that might be less obvious.

Jekyll *can* do a lot of the things you take for granted in most blogging software, but by default, it doesn't.  Do you want tags on your posts?  How about links to the previous and next posts?  Or for the blog to show ten posts at a time and a link to the next pageful, instead of blatting out everything at once?  Well,
get ready to start Googling and testing, because you can't just switch things
like that on from your control panel.

I suspect that if I compared the amount of time I spend twiddling Jekyll to the amount I spent troubleshooting Wordpress, I might not be all that much better
off.  But at least the Jekyll problems have been consistently fixable, which is more than I can say for Wordpress.  

I exported all the content from the old Wordpress blog as Jekyll files (though they'll need some clean-up for the new layout), so it's not gone forever, and I'm planning to add some small features here and there – prettier tags, for one thing, and automatic formatting of footnotes.  And there's been ongoing 
troubleshooting; every time someone looks at the site with a new kind of mobile device, they seem to discover some new bug.  (At one point, the videos played on some tablets but not others.)

So while Jekyll isn't less work for *me*, it does provide a more usable site
for my readers.  May you always outnumber me!
