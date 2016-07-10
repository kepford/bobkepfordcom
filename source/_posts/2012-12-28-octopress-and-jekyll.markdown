---
layout: post
title: "Octopress &amp; Jekyll"
date: 2012-12-28 17:44
comments: false
categories:
---

It's funny how I have come full circle. The first version of this site was written in static HTML and here we are headed into 2013 and my site is once again a static HTML based site. Why? Well, if you know me you know that I have been using and building [Drupal](http://drupal.org) sites for a good while so why the change?

First off, I have not abandoned [Drupal](http://drupal.org). I still think it is the best CMS we have. That said, I spend a ton of time working with [Drupal](http://drupal.org) and it is actually nice to use something else every now and then. I love [Drupal](http://drupal.org) but my site had been lagging behind on security updates. I'd rather spend my time writing blog posts. My favorite "feature" of [Jekyll](http://jekyllrb.com/) is that you don't need a database. No more security updates, no more hassles.

##Why Jekyll?

I'm a geek and love working on the commandline and with text files. I've been looking at different static site generators and I settled on Jekyll. [Jekyll](http://jekyllrb.com/) is written in Ruby and deployment is really easy. There are several ways you can host your [Jekyll](http://jekyllrb.com/) site including Github pages but since it produces static files I've opted for rsync the site to my server. When I want to update my site or write a post I simply open my text editor and go to town. When I'm done I type <code>rake generate</code> and then <code>rake deploy</code>. My entire site including the content is in a Git repo as well.

##Why Octopress?

I looked at [Jekyll](http://jekyllrb.com/) but had been hearing good things about [Octopress](http://octopress.org/) (by [Brandon Mathis](http://brandonmathis.com/) also check out [HSLpicker.com](http://hslpicker.com)) which is based on [Jekyll](http://jekyllrb.com/). [Octopress](http://octopress.org/) is basically a framework that is built on top of [Jekyll](http://jekyllrb.com/) but includes a ton of configuration that is not in [Jekyll](http://jekyllrb.com/).

###Octopress provides:

* A semantic HTML5 template
* A Mobile first responsive layout (rotate, or resize your browser and see)
* Built in 3rd party support for Twitter, Google Plus One, Disqus Comments, Pinboard, Delicious, * and Google Analytics
* An easy deployment strategy using [Github pages](http://pages.github.com/) or Rsync
* Built in support for POW and Rack servers
* Easy theming with [Compass](http://compass-style.org/) and [Sass](http://sass-lang.com/)
* A Beautiful [Solarized](http://ethanschoonover.com/solarized) syntax highlighting

I'm sure I could have went with Jekyll but my time was really limited. So far I'm really liking Octopress.

