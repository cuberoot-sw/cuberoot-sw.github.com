---
layout: post
title: "Resolved: create_yetting_class: undefined method 'each' for nil:NilClass"
date: 2013-03-12 16:59
comments: true
author: Prachi
categories:
redirect_from: 
  - /2013/03/12/issues-while-deploying-rails-app-on-production/

---

In this blog we are going through some issues which I faced while upgrading `gem twitter` and their fixes while deploying rails app on production.

Running a ruby script tweetstream.rb using `Yettings` in Rails `production environment` .

  while running a tweetstream.rb using Yettings in production with command :


While running script with Yettings, an error occurs for `config/enviornment` .


```ruby
  tweetstream.rb:11:in `require': cannot load such file -- /Users/prachi/work/cuberoot/rubybeats/aggregator/config/environment.rb (LoadError)
  from tweetstream.rb:11:in `<main>'
```


  To fix this error, added following code with using `File.join` in `tweetstream.rb` file .

```ruby
  root = File.expand_path(File.dirname(__FILE__))

  # added following code to require following files with correct path
  require File.join(root + '/..', "config", "environment")
```

<!-- more -->

Running daemon on rails production env by using `gem daemons-rails` .

  The file `config/daemons.yml` is added by installing `gem daemons-rails` .

  When I started a daemon  by running :

```ruby
$ bundle exec ruby lib/daemons/tweetstream_ctl start
```

  Got an error like :

```ruby
...
releases/log/tweetstream.rb.pid file not found.
...
```
  So, to load the correct log file set  `dir` option in `config/daemons.yml` to correct log path.

```ruby
  dir_mode: script
  # set a correct log directory path here
  dir: ../../../current/log
  multiple: false
  backtrace: true
  monitor: true
  ontop: false
```
