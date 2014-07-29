---
layout: post
title: "Integrating Fonts In Rails Application"
date: 2014-05-07 12:26:30 +0530
author: Deepali
---

In this blog post we are going to integrate Fonts in our Rails
Application.

#####1. Add your font files under `/assets/fonts` folder.

#####2. Add following line in `config/application.rb` file within your Application class : 

```ruby
  config.assets.paths << Rails.root.join("app", "assets", "fonts")
```

#####3. Change Fonts path in css file.

  Your Default CSS file contain the fonts path as follows :

```css
  @font-face {
    font-family: 'FontAwesome';
    src:url("../font/fontawesome-webfont.eot?v=3.2.1");
    src:url("../font/fontawesome-webfont.eot?#iefix&v=3.2.1") format("embedded-opentype"),
        url("../font/fontawesome-webfont.woff?v=3.2.1") format("woff"),
        url("../font/fontawesome-webfont.ttf?v=3.2.1") format("truetype"),
        url("../font/fontawesome-webfont.svg#fontawesomeregular?v=3.2.1") format("svg");
    font-weight: normal;
    font-style: normal;
   }
  
```
  We need to change this to :

```css
  @font-face{
    font-family:'FontAwesome';
    src:url("/assets/fontawesome-webfont.eot?v=3.2.1");
    src:url("/assets/fontawesome-webfont.eot?#iefix&v=3.2.1")format("embedded-opentype"),
        url("/assets/fontawesome-webfont.woff?v=3.2.1") format("woff"),
        url("/assets/fontawesome-webfont.ttf?v=3.2.1") format("truetype"),
        url("/assets/fontawesome-webfont.svg#fontawesomeregular?v=3.2.1") format("svg");
    font-weight:normal;
    font-style:normal}
  }
  
```


#####4. Restart you server.

Thats it you are Done !
