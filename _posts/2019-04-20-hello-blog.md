---
layout: post
title:  "Hello Blog !!!"
date: 2019-04-20
tags: [jekyll, blog]
excerpt_separator: <!--more-->
---
Hi, dronjax here.

This is the very first post.

I am using [Jekyll][jekyll-gh]{:target="_blank"} to construct this blog.

It required some experimentation for constructing the blog.
<!--more-->
For installing you can just use these steps:
{% highlight bash %}
gem install bundler jekyll
jekyll new your-site
cd your-side
bundle install
bundle update
{% endhighlight %}

The tricky part was to setup the theme.
I need to:
- Change the *_config.yml* for the key *theme*.
- Copy default layout from [Cayman][cayman-default]{:target="_blank"} source.
- Add the layout for post.

The details of the change can be checked out on my [Github Page][dronjax-gh]{:target="_blank"}.

[jekyll-gh]: https://github.com/jekyll/jekyll
[cayman-default]: https://github.com/pages-themes/cayman/blob/master/_layouts/default.html
[dronjax-gh]: https://github.com/dronjax/dronjax.github.io
