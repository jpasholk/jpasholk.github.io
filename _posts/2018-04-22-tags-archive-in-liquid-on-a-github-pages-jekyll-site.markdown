---
layout: post
title: "Tags Archive in Liquid on a GitHub Pages Jekyll Site"
date: "2018-04-22 15:09:38 -0700"
tags: [webdev, Jekyll, GitHub-Pages, Liquid, forloops]
---

## What is a blog without tags and an archive page?

![Post Tags - via Undraw](/img/posts/post-tags.png)

When I started on this site, I wasn't looking forward to figuring out tags logic and building the archive page to show them. I had tried using other Jekyll themes, and sometimes their tags would work and sometimes they wouldn't.

<!--more-->

Because everyone has a different way of doing things, sometimes it's hard to follow the logic of why things work and why they don't. It took me sitting down and watching a lynda.com video series on Jekyll to understand how tags worked. At least enough to implement a simple archive page sorted by tags. I still have a lot to learn about Jekyll, but this is how I achieved my tags archive.

Something worth noting is that James Williamson, the author of the video series I watched, mentioned a [blog post](https://blog.lanyonm.org/articles/2013/11/21/alphabetize-jekyll-page-tags-pure-liquid.html){:target="_ blank"} by Michael Lanyon about alphabetizing Jekyll page tags in pure Liquid. Give it a read if you have a few minutes, you won't be disappointed.

From what I learned, you need four things to get tags working on a Jekyll site:

1. A tags array in at least one post's front matter.
2. Some code to capture and sort those tags.
3. Other code to loop through them and print them to the page.
4. An archive page to display them.

### Capturing the tags and building the tag cloud

![Tag Cloud - via Undraw](/img/posts/tag-cloud.png)

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

Jekyll will store all the tags you specify in the front matter of your posts in the `site.tags` object. Any time you add a new tag to a post, they will be added automatically upon rebuilding the site, this is super useful for creating the tag page, although it's not very straightforward.

In the code above, the first part will create a variable called `site_tags` using `capture`. It will then loop through the tags object and grab the name of each tag and assign it as a variable called `tag`. Since each tag in the tags object has two properties, a name and a list of posts, it does this by using the `first` filter which grabs the first property. It then creates a new array called `sortedTags` which takes the names from `site_tags` and alphabetizes them and splits them up with a comma, unless it's the last entry, then it leaves the comma off.

The second will loop through the array `sortedTags` and prints the tag name along with the number of posts associated with it. It seems pretty complicated, but once you work it out, it's not that bad.

### Creating the archive page

![Archive Page - via Undraw](/img/posts/archive-page.png)

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

Now I need to loop through the `sortedTags` array to display our tags in the archive page, and then loop through the posts associated with each tag. I’ll use another forloop, with another forloop nested inside, this will create a list of tags with a list of posts under each one.

In the code example above, the first loop prints the name each of the tags in a header, while the second loops through the posts for each tag and prints the date and then the title with a link. Again, it’s not the most natural thing to wrap your head around at first. The great thing about Jekyll is it sort of lets you tiptoe into programming concepts without being bogged down with semantics.

### Showing the tag cloud in the blog roll

{% highlight html %}
{% raw %}

{% for tag in post.tags %}
<a href="{{ site.baseurl }}/archive/index.html#{{ tag | cgi_escape }}" title="Pages tagged {{ tag }}" rel="tag">{{ tag }}</a>{% unless forloop.last %}, {% endunless %}
{% endfor %}

{% endraw %}
{% endhighlight %}

Creating the tag cloud for each post is a bit trickier, you can’t use the same logic as the tag cloud on the archive page. It has only to display the tags that are in the post it’s shown on. I can’t loop through `sortedTags` because it will just give me the same global sorted tag list.

To access the post tags, I’ll need to use `post.tags` and loop through that. Also, the `href` value has to be relative to jump to the archive page and then land on the right section.

Another thing to mention is the `cgi_escape` filter you may have seen. It strips the spaces out of the tag and replaces it with a dash, this is, so the URL’s of the website still work in all circumstances.

## Thanks for stopping in!

![Designer - via Undraw](/img/posts/designer.png)

I hope you like this post!

I will try and get a post out about once a week or so about my process of building this site and eventually turning it into a theme. There is a lot more to cover so stay tuned for more updates!
