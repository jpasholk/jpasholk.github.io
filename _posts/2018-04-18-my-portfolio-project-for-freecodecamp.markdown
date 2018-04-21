---
layout: post
title: "Building my Portfolio Project with Jekyll and UiKit"
date: "2018-04-18 07:29:19 -0700"
tags: [freeCodeCamp, webdev, UiKit, portfolio-project, Jekyll, GitHub-Pages]
---

![Website Mockup - via Undraw](/img/posts/web-mockup.png)

When I first started my journey on freeCodeCamp, I was really looking forward to the [portfolio project](https://www.freecodecamp.org/challenges/build-a-personal-portfolio-webpage){:target="_ blank"}. I had built a few of my own websites before using Wordpress.org, but it always left me wanting more.

<!--more-->

As amazing as the Wordpress ecosystem and community are, I couldn’t help but think that it was just too bloated and heavy for my use case. There is a decently steep learning curve when going from ‘plug-in juggler’ to ‘theme developer’. I’ve also heard that PHP can be a pain to learn, especially as your first language.

I’d like to get there someday soon, but Wordpress will have to wait.

## Enter static site generators

A few months before, a friend of mine recommended taking a look at [Jekyll](https://jekyllrb.com/){:target="_ blank"}, a popular, blog-aware, static site generator. I’d been wanting to actually learn web development and had an idea for a simple blog project. With a decent understanding of HTML5, a few CSS essentials, and some help from my friend I was able to get [inthecomments](https://inthecomments.today/){:target="_ blank"} up and running in a few days using a Jekyll theme and GitHub Pages.

The idea behind the blog was to be a project that I could finish quickly while learning the basics of setting up a Jekyll site. I hope to extend the readme one day soon to encourage people that are new to GitHub to learn how to make pull requests. All they would need to do is write a microblog post about something funny or interesting they found online somewhere (most likely Reddit)and submit a pull request to the [repo](https://github.com/jpasholk/inthecomments){:target="_ blank"}.

CloudCannon has a really great [tutorial series](https://learn.cloudcannon.com/){:target="_ blank"} on Jekyll. Check it out if you have some time, you'll learn something guaranteed.

![Site-build - via Undraw](/img/posts/site-build.png)

## Time to build something

I decided on using a framework to build my project rather than vanilla CSS. Somewhere in between browsing through web development subreddits and medium stories, I stumbled on a framework called [UiKit](https://getuikit.com/){:target="_ blank"}. I really liked what I saw when reading through the documentation. It offers a lot of the same functionality as other frameworks but just looks better, in my opinion (of course).

### Starting from the top

I started with the nav bar. I knew that I wanted to keep it simple, just a text logo on the left and a few pages of navigation on the right. Heading over into Navbar section of UiKit’s documentation, it says:

>The Navbar component consists of a navbar container, the navbar itself and one or more navigations.

#### UiKit markup for a simple Navbar with a text logo on the left

{%  highlight html %}
<nav class="uk-navbar-container" uk-navbar>
  <div class="uk-navbar-left">
    <a class="uk-navbar-item uk-logo" href="#">Logo</a>
  </div>
  <div class="uk-navbar-right">
    <ul class="uk-navbar-nav">
      <li class="uk-active"><a href="#">Active</a></li>
      <li><a href="#">Item</a></li>
      <li>
        <a href="#">Parent</a>
        <div class="uk-navbar-dropdown">
          <ul class="uk-nav uk-navbar-dropdown-nav">
            <li><a class="uk-nav-header" href="#">Item</a></li>
          </ul>
        </div>
      </li>
    </ul>
  </div>
</nav>
{% endhighlight %}

This is the basic skeleton I put together to start with. While a good start, it is not responsive. When viewed on mobile, the navigation links do scale with the site but they don’t turn into any type of menu icon with a drop-down. I researched how to do this with UiKit and found that there really isn’t an elegant solution like Bootstrap. The [documentation](https://getuikit.com/docs/offcanvas#nav-in-off-canvas){:target="_ blank"} recommends using an Off Canvas component for mobile navigations. This is more of a sidebar style, which wouldn’t fit with the design I was going for.

I started thinking about it in another way after going over the examples in their documentation and experimenting a bit. I read somewhere along the line that a somewhat dirty implementation is to have two menus and show only the one you want with CSS, it’s not perfect but it will get the job done.

UiKit uses responsive visibility classes to display or hide elements on different devices, so I essentially had to create two div’s with the `uk-navbar-right` class, with the first also having the class `uk-visible@s` and the other `uk-hidden@s`. This makes it so the first one only shows on devices that are larger than `640px` with the other being hidden and vice versa.

#### You end up with something like this

{%  highlight html %}
  <nav class="uk-navbar-container" uk-navbar>
    <div class="uk-navbar-left">
      <a class="uk-navbar-item uk-logo" href="#">Logo</a>
    </div>
    <div class="uk-navbar-right uk-visible@s">
      <ul class="uk-navbar-nav">
        <li class="uk-active"><a href="#">Active</a></li>
        <li><a href="#">Item</a></li>
        <li>
          <a href="#">Parent</a>
          <div class="uk-navbar-dropdown">
            <ul class="uk-nav uk-navbar-dropdown-nav">
              <li><a class="uk-nav-header" href="#">Item</a></li>
            </ul>
          </div>
        </li>
      </ul>
    </div>
    <div class="uk-navbar-right uk-hidden@s">
      <ul class="uk-navbar-nav">
        <li class="ad_navicon_toggle"><a href="" uk-icon="icon: menu"></a>
          <div uk-dropdown class="uk-border-rounded">      
            <ul class="uk-nav uk-dropdown-nav">
              <li class="uk-nav-header"><a href="#">Item</a></li>
              <li class="uk-nav-header"><a href="">Item</a></li>
              <li class="uk-nav-header"><a href="">Item</a></li>
            </ul>
          </div>
        </li>
      </ul>
    </div>
  </nav>
{% endhighlight %}

####  Here is the CodePen if you would like to try it out for yourself

<p data-height="354" data-theme-id="dark" data-slug-hash="rvVPpM" data-default-tab="html,result" data-user="jpasholk" data-embed-version="2" data-pen-title="UIKit Navbar Mobile" data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/jpasholk/pen/rvVPpM/">UIKit Navbar Mobile</a> by Josh Pasholk (<a href="https://codepen.io/jpasholk">@jpasholk</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

That is about as in depth about my navbar as I’d like to get, for now. I ended up included some [Liquid](https://help.shopify.com/themes/liquid/basics){:target="_ blank"} logic to loop through links in a data file that I’ll go over in a later post. I plan on turning this into a theme, and I’ve seen a lot of themes do stuff like this to make it easier for people who aren’t great with HTML.

### Laying out the home page with sections

![Home layout - via Undraw](/img/posts/home-layout.png)

Next, I needed to start laying out the wireframe of the homepage. UiKit has a component called `Section` which nicely lays everything out in a grid.

From their documentation:

>Create horizontal layout sections with different background colors and styles.

{% highlight html %}
  <div class="uk-section uk-section-default">
    <div class="uk-container">
      <h3>Section Default</h3>
      <div class="uk-grid-match uk-child-width-1-3@m" uk-grid>
        <div>
          <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor.</p>
        </div>
        <div>
          <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor.</p>
        </div>
        <div>
          <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor.</p>
        </div>
      </div>
    </div>
  </div>
{% endhighlight %}

This is pretty much perfect as it already does a bunch of work with padding and margin, etc. I only had to adapt its structure to fit my needs for the homepage. I wanted a large header with an image in the background and text in front. Following that a few more sections for the Projects and Contact pages, and one for the footer.

All in all, it was a fairly quick process, after a bit of troubleshooting I was done with my final layout in about a day of tinkering. Any changes made after that were tiny adjustments or refactoring the code into Jekyll includes or layouts. It took me an additional weekend to figure out the tag logic and another three days or so to implement [Algolia](https://community.algolia.com/jekyll-algolia/){:target="_ blank"} search. I will go over those in separate posts at a later date.

#### CodePen

<p data-height="373" data-theme-id="dark" data-slug-hash="aEVKBQ" data-default-tab="html,result" data-user="jpasholk" data-embed-version="2" data-pen-title="UIKit Sections" data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/jpasholk/pen/aEVKBQ/">UIKit Sections</a> by Josh Pasholk (<a href="https://codepen.io/jpasholk">@jpasholk</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## Thanks for reading

![Designer - via Undraw](/img/posts/designer.png)

Thanks for stopping by!

I will try and get a post out about once a week or so about my process of building this site and eventually turning it into a theme. There is a lot more to cover and I didn’t want to make this post too big so stay tuned for more updates!

Some things I’ll cover are:

* Looping through blog posts.
* Looping through tags for a tag cloud.
* Using data files for navigation.
* Creating an archive page sorted by tag.
* Integrating Algolia search. (Super cool!)
* Adding an active state to nav items.
* Issues or problems that I can't seem to solve.

*Credit: Cute images provided by [Undraw](https://undraw.co){:target="_ blank"}*{:class="uk-article-meta"}
