---
layout: post
title: "Tags Archive in Liquid on a GitHub Pages Jekyll Site"
date: "2018-04-22 15:09:38 -0700"
tags: [webdev, Jekyll, GitHub-Pages, Liquid, forloops]
---

## What is a blog without tags and an archive page?

![Post Tags - via Undraw](/img/posts/post-tags.png)

When I started building this site I wasn't looking forward to tags and the archive page to show them. I had tried using other Jekyll themes and sometimes their tags would work and sometimes they wouldn't.

<!--more-->

Because everyone does things a little differently it is hard to follow the logic of why things work and why they don't. It actually took me sitting down and watching a lynda.com video series on Jekyll to understand how tags worked. At least enough to implement a simple archive page sorted by tags. I still have a lot to learn about Jekyll, but this is how I achieved my tags archive.

Also, worth noting is that James Williamson, the author of the video series I watched, mentioned a [blog post](https://blog.lanyonm.org/articles/2013/11/21/alphabetize-jekyll-page-tags-pure-liquid.html){:target="_ blank"} by Michael Lanyon about alphabetizing Jekyll page tags in pure Liquid. Give it a read it you have a few minutes, you wont be disappointed.

### Capturing the tags and building the tag cloud

{% highlight html %}
{% raw %}

{% capture site_tags %}
  {% for tag in site.tags %}
    {{ tag | first }}{% unless forloop.last %},{% endunless %}
  {% endfor %}
{% endcapture %}

{% assign sortedTags=site_tags | split:',' | sort %}

{% for tag in sortedTags %}
  <span class="tags">
    <a href="#{{tag | cgi_escape}}">{{ tag }} [{{ site.tags[tag].size }}]</a>
    {% unless forloop.last %},{% endunless %}
  </span>
{% endfor %}

{% endraw %}
{% endhighlight %}

Jekyll will store all the tags you specify in the front patter of your posts in the `site.tags` object. Any time you add a new tag to a post they will be added automatically. This is super useful for creating the tag page, although it's not really straightforward.

The first part will create a variable called `site_tags` using `capture`. It will then loop through the tags object and grab the name of each tag and assign it as a variable called `tag`. Since each tag in the tags object has two properties, a name and a list of posts, it does this by using the `first` filter which grabs the first property. It then creates a new array called `sortedTags` which takes the names from `site_tags` and alphabetizes them and splits them up with a comma, unless it's the last entry, then it leaves the comma off.

The second will loop through the array `sortedTags` and prints the tag name along with the number of posts associated with it. It seems pretty complex, but once you work it out it's not that bad.

### Creating the archive page list by tag

{% highlight html %}
{% raw %}

{% for tag in sortedTags %}
<div>
  <h3 id="{{tag | cgi_escape}}" class="tag-name"><span>{{tag}}</span></h3>
    <ul class="tag-list">
    {% for post in site.tags[tag] %}
      <li>
        <time itemprop="dateCreated" datetime="{{post.date}}">{{post.date | date: "%b %d, %Y "}}</time> <a href="{{site.baseurl}}{{post.url}}" rel="bookmark" title="Link to {{site.baseurl}}{{post.url}}">{{post.title}}</a>
      </li>
    {% endfor %}
    </ul>
  </div>
{% endfor %}

{% endraw %}
{% endhighlight %}

Now I need to loop through the `sortedTags` array in order to display our tags in the archive page, and then loop through the posts associated with each tag. I'll use another forloop, with another forloop nested inside. This will essentially create a list of tags with a list of posts under each one.

The first loop prints the name of the tag in a header, while the second loops through the posts for each tag and prints the date and then title with a link. Again, it's not the easiest thing to wrap your head around.

## Showing the tag cloud in the blog roll
