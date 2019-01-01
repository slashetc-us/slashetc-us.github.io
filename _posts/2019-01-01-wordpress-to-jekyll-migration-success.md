---
layout: post
title: Wordpress to Jekyll Migration Success!!
date: 2019-01-01 11:50:29 -05:00
author: Michael J. Ford
type: post
published: true
status: publish
---

### Hello Markdown World!!
{: .center-text}  

![](https://jekyllrb.com/img/logo-2x.png){: .center-image}

Good bye Wordpress (and good riddens), hello [Jekyll](https://jekyllrb.com/)!!
I've finally jumped off the Wordpress bandwagon, leaving all it's complexities and
constant vulrnabilities behind in favor of Jekyll. This has allowed me to publish
my website іn a static manner without constantly updating my site to combat the
never ending flow of exploits, vulrnabilities, etc. that come with Wordpress.  

The change however does not come without cost. There are a few luxuries Wordpress
affords, one thing is it has wonderfull tools to manage and format your posts,
with a WYSIWYG style editor in a browser. This method is nice for someone who
just want's to get content up on a site (and in 2014 when I purchased my domain
that was exactly what I was looking to do.) Another it theme management, themes
in Jekyll, don't exactly play nice when you're first starting out. Wordpress
themes mostly *just work*, at least in way that it's simply a few button clicks
to make it look the way you want it. In Jekyll it took me longer to find a theme
that I liked and didn't break the site than it did to convert my Wordpress posts
over.  

However, where Jekyll has more manual tasks in comparison to Wordpress - they
can be scripted. The best part of Jekyll I've found is one you have it all
setup - you can manage it completely from the terminal. Not something you can
say about Wordpress. This post I typed in Markdown, completely from a terminal
running vim on my laptop. When it's ready, it's simply commited to my GitLab
instance and CI tasks are automatticly kicked off to build the site and push
the content off to either a Dev VM, or out to production (where you're reading
this now.) One of the biggest plusses however, when developing the site/adding
posts/etc. Jekyll supports building and hosting the site locally, so you can
see your changes locally - in real time - without deploying to dev or prod.
Simply run `bundle exec jekyll serve` in the root of the repo and point your
browser at `http://localhost:4000`. The site will automatticly be rebuilt on
file changes.

Writing in markdown was another hurdle, a small one but it existed. While re-building
this site and working with it more closely than a quick README.md or wiki page I
found it extremely easy to pick up and write in. Cheatsheets are also nice to have
at times, there's a nice one in Resources below.

At the end of the day, I'm glad I took the time and effort to migrate over to
Jekyll. Not only will it save be management time in the future, but I learned
new things at the same time. Maybe I'll even want to post more again.

#### Resources:
{: .center-text}  
[Jekyll Documentation](https://jekyllrb.com/docs/)
[Hacker-Blog Theme](https://github.com/tocttou/hacker-blog)
[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
{: .center-text}
