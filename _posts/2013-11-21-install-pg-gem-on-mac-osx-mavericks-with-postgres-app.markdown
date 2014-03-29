---
layout: post
title: "Install pg gem on Mac OSX Mavericks with Postgres.app"
date: 2013-11-21 09:21
comments: true
categories: 
author: Girish
---

When I was trying to install `pg` gem on OSX Mavericks it failed with
following error

```
gem install pg
.
.
.
checking for libpq-fe.h... no
Can't find the 'libpq-fe.h header
```

I am using [Postgres.app](http://postgresapp.com/) on my mac. Turns out
the installer cannot locate the include headers, and it's a easy fix.

```
export CONFIGURE_ARGS="with-pg-include=/Applications/Postgres.app/Contents/MacOS/include"
gem install pg
```

The gem installs successfully. Hope this helps someone.
