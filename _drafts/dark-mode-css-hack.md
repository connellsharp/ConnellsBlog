---
layout: post
title: Retrofitting Dark Mode CSS
tags: this analytics jekyll
---

```
// Dark mode
@mixin dark-mode {
  @media (prefers-color-scheme: dark) {
    @content;
  }
}
```


```
@mixin color-normal {
  color: $darkGray;

  @include dark-mode {
    color: $lighterGray;
  }
}
```

```
.invert-dark-imgs img {
  @include dark-mode {
    filter: invert(100%) hue-rotate(180deg) contrast(0.73) saturate(1.5);
  }
}
```