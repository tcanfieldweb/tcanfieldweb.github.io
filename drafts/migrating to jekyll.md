constant frustrations with Wordpress - themes are an unmaintainable tissue of hacks
there's a reason this is a job skill
constant odd behavior from CSS

recommended Jekyll
generates static pages
This makes sense for high-traffic site, since pages aren't constantly calling databases
And it's quicker for users for the same reason
Few author blogs have to deal with that kind of load
But it's not Wordpress

All the rage comes from the setup - like most open source projects, the documentation is not all that great

Heard it was based on Ruby, but while that's technically true, didn't write any Ruby setting it up
Getting it running is largely a method of tracing through the different files and comparing them to the webpages to figure out where various parts of the content come from

Forked [jekyll-now[(https://github.com/barryclark/jekyll-now) from GitHub; seemed commonly used and was similar to what I had in mind
Jekyll-now isn't set up using Bootstrap - might not be a big deal, but wanted to be usable on mobile
There's a jekyll-bootstrap project, but it's not based on jekyll-now, and it's ugly
Added Bootstrap support using this method: (link if I can find it)

Variables - in _config.yml and the YAML front matter at the beginning of posts and templates.
So there's no Ruby - what is there?  Two (2) different markup languages, because one isn't enough
Markdown - pretty straightforward; add the most common HTML tags by just adding text, and the less common can be written as HTML if you need them.
Liquid - not too complex; puts in the site variables
Relies on one or more templates
Anything repeated - the navbar, say - is generated using the template

Doesn't, by default, support tagging - which I think is how a significant number of people who've discovered a post they like decide if they want to dig deeper
And good examples were thin on the ground - perhaps because so many people are using GitHub to host, and GitHub doesn't run plugins
Worst, all Google roads seemed to link to the same couple of pages.
(Yes, there's a plugin-free version, but it seemed to either be missing a step, or to make assumptions about your  base setup that weren't true for jekyll-now.)
In fact a number of pages I read about tagging had similar problems - they didn't work as-is, and I hadn't learned enough about Jekyll in my first couple of hours to get them working.  I wasn't about to invest a ton of time in something that might not do what I wanted when I was done, when I could be learning something I actually would use later.

The [jekyll-tagging](https://github.com/pattex/jekyll-tagging) plugin turned 
out to do what I wanted.  (I didn't have my heart set on hosting on GitHub in 
the first place, so using a plugin was not a big deal).  The instructions are 
fine - the one problem I ran into came from changing a file and not saving it, 
which isn't Eilermann and Wille's fault.  (On the bright side,
tracking down even a silly problem will teach you a lot about a system.)

I'm going to migrate my old blog content over here, or at least some of it, 
so it'll be more like a migration where some of the birds didn't make it 
and some new ones hatched along the way.  There's existing code to
[import Wordpress blogs](http://import.jekyllrb.com/docs/wordpress/) - we'll see how that works.
wp-config.php has database password

