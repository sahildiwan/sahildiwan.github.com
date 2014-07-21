---
layout: post
title:  "How to make this blog using Jekyll and deploy it on Github pages"
date:   2014-07-17 00:08:00
categories: jekyll update
---

This blog post is about creating this exact blogging site using the simple, blog-aware, static site generator, [Jekyll][1]. Jekyll is the engine behind [Github Pages][2], the place you will be deploying for free once finished. I like using Jekyll because of the fact that I can write static posts in [markdown](http://en.wikipedia.org/wiki/markdown) instead of HTML.

Notes: 

- The source code for this project can be found [here][3].
- I am running Mac OS X and this tutorial (at least installation) will be specific. If you are on a different OS go [here][4].
- I couldn't post a lot of code here because Jekyll was automatically rendering some of the dynamic code I wanted shown. Therefore, I have linked those parts to the specific files in my Github repo.
- Remember to swap out my content for yours :)

Let's get started.

`1.)` Installation and setup.

Assuming you have Ruby/[RubyGems][5] installed, and are running Mac OS X, run the following command in your terminal.

{% highlight bash %}
$ gem install jekyll
$ gem install rdiscount
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

This will create a `_site` folder within your project, ignore it. Jekyll already gives you a simple great looking site, if you are happy with it and just want to start writing blog posts skip to step `4`.

`2.)` Configuration and adding/editing files

First, edit your `_config.yml` file to look like this:

{% highlight text %}
markdown: rdiscount
pygments: true
permalink: /posts/:title
rdiscount:
  extensions: [smart]
{% endhighlight %}

Then, edit the `feed.xml` file to look like the the code found [here][7].

*Reminder: couldn't post the code here because Jekyll was automatically rendering some of the dynamic code I wanted shown. #lookingoutforyou #inception

Now, edit the `index.html` file to look like this:

{% highlight html %}
---
layout: default
title: Add a title
---

<p>Write a nice short bio about yourself 
here, it will be shown on the home page.</p>
{% endhighlight %}

Next, create a file called `posts.html` and add the code found [here][8].

Lastly, edit the `.gitignore` file and add `.DS_Store` below `_site`. You don't have to, but please just do it.

`3.)` Adding static front-end files

Now we need to add the HTML, CSS, and JavaScript files used for this blog site.

First, remove the folder called `_includes`, you won't need it. Next, go into `_layouts` and edit the `default.html` file to look like the code found [here][9]. Then, edit the `post.html` file to look like th code found [here][10]. Delete `page.html`.

We don't actually need to add any CSS or JavaScript for this blogging site because we are linking the style-sheet and some JavaScript found [here][11] and [here][12].

Run

{% highlight bash %}
$ jekyll serve
{% endhighlight %}

And you should see your new blog at [http://0.0.0.0:4000/][6]. All we have left now to do is write some posts and deploy to [Github Pages][2]. As you can see, Jekyll has already made your first post for you.

`4.)` Writing blog posts in Markdown

All posts you write for your Jekyll site must go in the `_posts` folder. There is one other rule, you must name your `.markdown` files using the convention `YYYY-MM-DD-name-of-post.markdown`.

At the beginning of each of your `.markdown` files make sure to add the following:

{% highlight text %}
---
layout: post
title:  "Title of blog post"
date:   YYYY-MM-DD XX:YY:ZZ
categories: jekyll update
---
{% endhighlight %}

To learn what other fancy things you can do with markdown, go [here][13] or look at my [source code][3].

`5.)` Deploying on Github Pages

Coming `soon`

[1]: http://jekyllrb.com/ 
[2]: https://pages.github.com/
[3]: https://github.com/sahildiwan/sahildiwan.github.com
[4]: http://jekyllrb.com/docs/installation/
[5]: http://rubygems.org/pages/download
[6]: http://0.0.0.0:4000/
[7]: https://github.com/sahildiwan/sahildiwan.github.com/blob/master/feed.xml
[8]: https://github.com/sahildiwan/sahildiwan.github.com/blob/master/posts.html
[9]: https://github.com/sahildiwan/sahildiwan.github.com/blob/master/_layouts/default.html
[10]: https://github.com/sahildiwan/sahildiwan.github.com/blob/master/_layouts/post.html
[11]: https://github.com/themes/minimal/stylesheets/styles.css
[12]: https://github.com/themes/minimal/javascripts/scale.fix.js
[13]: https://github.com/adam-p/markdown-here/wiki/Markdown-Here-Cheatsheet
