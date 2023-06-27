---
layout: post
title: Site Analytics Without a Cookie Banner
tags: this analytics jekyll
---

On the way to NDC London this year, I sat on the train and wondered, how many people actually read this blog? It's just [a static site hosted by GitHub](https://github.com/connellsharp/ConnellsBlog), so I don't see any numbers on the back end. I needed something that runs in the browser and sends data back to me. Of course, I installed Google Analytics. The problem is...

GA requires a cookie banner. I hate those stupid banners. Some sites make them hilariously intrusive to entice you into accepting. Some of them use a fake timeout to "save your preferences" if you dare to reject being tracked. You'll find some that grant so much customisation of your cookie preferences that the "don't spy on me" button is hidden a full page scroll away.

I thought I'd be the change I want to see in the world. My cookie banner's just going to be a friendly, unobtrusive message at the bottom of the page with two buttons.

> Sorry, I use analytical cookies. [That's fine] [Please don't]

The problem with this? You guessed it. No-one clicks it.

GA has a "consent mode" you can use if users don't accept cookies, but it turns out that doesn't even track hits, which is the main thing I'm interested in. I'm not really sure what use that has.

## Request Metrics

By sheer coincidence, I bumped into [Todd Gardner](https://twitter.com/toddhgardner) there, who suggested I try [Request Metrics](https://requestmetrics.com/). It doesn't use cookies and is GDPR compliant. I can get the data I want and no-one needs to be coerced into anything.

Installing it is just adding this script tag.

```
{% raw %}
<script async src="https://cdn.requestmetrics.com/agent/current/rm.js" data-rm-token="{{ site.request_metrics }}"></script>
{% endraw %}
```

And of course, `request_metrics` is just a variable in [my `_config.yml` file](https://github.com/connellsharp/ConnellsBlog/blob/master/_config.yml).

## What do I get?

Turns out people do read this blog! I can see which pages are most popular, which countries the views are coming from, which devices people are using, and how they found me. Everything I wanted from GA, bit without the dreaded cookie banner.

Pretty encouraging! Maybe I'll get my act together and write some more posts. Maybe.