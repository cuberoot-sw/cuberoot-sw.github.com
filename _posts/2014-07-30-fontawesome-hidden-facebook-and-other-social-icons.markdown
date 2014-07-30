---
layout: post
title: "FontAwesome hidden facebook and other social icons"
date: 2014-07-30 13:31:53 +0530
author: Girish
---

Yesterday, I spent nearly an hour fixing the mysterious disappearance of facebook, twitter etc. FontAwesome icons.

It was working locally, but on production server the social icons were hidden.

Turns out, "Adblock Plus" hides any html elements with class names containing text facebook, twitter, linkedin or github.

I ended up modifying the `font-awesome.css` file and renaming the class names, since I was only using five icons.

```css
.icon-lnkd:before             { content: "\f0e1"; }
.icon-gp:before               { content: "\f0d5"; }
.icon-tw:before               { content: "\f099"; padding-left: 2px; }
.icon-f1b:before              { content: "\f09a"; padding-left: 6px; }
.icon-gh:before               { content: "\f09b"; }
```

For more details see this FontAwesome [issue](https://github.com/FortAwesome/Font-Awesome/issues/1799)

Hope this helps someone!
