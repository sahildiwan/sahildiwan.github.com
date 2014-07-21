---
layout: post
title:  "How to make this blog using Jekyll and deploy it on Github pages"
date:   2014-07-17 00:08:00
categories: jekyll update
---

This blog post is about creating this exact blogging site using the simple, blog-aware, static site generator, [Jekyll][1]. Jekyll is the engine behind [Github Pages][2], the place you will be deploying for free once finished.  

Notes: 

- The source code for this project can be found [here][3].
- I am running Mac OS X and this tutorial (at least installation) will be specific. If you are on a different OS go [here][4].

So let's get started.

`1.)` Installation and setup.

Assuming you have Ruby/[RubyGems][5] installed, and are running Mac OS X, run the following command in your terminal.

{% highlight bash %}
$ gem install jekyll
{% endhighlight %}

*Add a `sudo` in front of it if you need root permission.

Then,

{% highlight bash %}
$ cd your/fav/directory
$ jekyll new sexyblog
{% endhighlight %}

This will create everything you need to start writing your blog posts.

Run the following to check out your Jekyll site locally at [http://0.0.0.0:4000/][6],

{% highlight bash %}
$ cd sexyblog
$ jekyll serve
{% endhighlight %}

Jekyll already gives you a simple great looking site, if you are happy with it and just want to start writing blog posts skip to step `4`.

`2.)` Configuration and adding/editing files

First, edit your `_config.yml` file to look like this:

{% highlight yml %}
markdown: rdiscount
highlighter: true
permalink: /posts/:title
rdiscount:
  extensions: [smart]
{% endhighlight %}

Then, delete the `feed.xml` file and create a new XML file called `atom.xml` with the code found [here][7].


[1]: http://jekyllrb.com/ 
[2]: https://pages.github.com/
[3]: https://github.com/sahildiwan/sahildiwan.github.com
[4]: http://jekyllrb.com/docs/installation/
[5]: http://rubygems.org/pages/download
[6]: http://0.0.0.0:4000/
