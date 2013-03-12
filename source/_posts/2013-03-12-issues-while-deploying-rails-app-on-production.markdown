---
layout: post
title: "Issues while deploying rails app on production"
date: 2013-03-12 16:59
comments: true
author: Prachi
categories:
---

#### In this blog we are going through some issues which I faced and their fixes while deploying rails app on production.

* Running a ruby script using `Yettings` in Rails `production environment` .

  while running a ruby script using Yettings in production with command :

```ruby
$ bundle exec ruby my_ruby_script.rb
```

  But following error occured :
```ruby
.....
in 'create_yetting_class: undefined method 'each' for nil:NilClass (NoMethodError)'
.....
```

  Run my_ruby_script in production enviornment with specifying `RAILS_ENV=production` .

```ruby
$ RAILS_ENV=production bundle exec ruby my_ruby_script.rb
```

* While running script with Yettings, an error occurs for `config/enviornment` .

```ruby
  ...
  cannot find 'config/enviornment'
  ...
```

  To fix this error, added following code with using `File.join` in `my_ruby_script.rb` file .

```ruby
  root = File.expand_path(File.dirname(__FILE__))

  require File.join(root + '/..', "config", "environment.rb")

  ## added following code to require following files with correct path

  require File.join(root, "helper.rb")
  require File.join(root, "monkey_patches.rb")

  $mylogger = Logger.new(File.join(root + '/..', "log", "xxtweetstream.rb.log"))
```

* Running daemon on rails production env by using `gem daemons-rails` .

  The file `config/daemons.yml` is added by installing `gem daemons-rails` .

  When I started a daemon  by running :
```ruby
$ bundle exec ruby lib/daemons/my-daemon-script_ctl start
```

  Got an error like :
```ruby
...
releases/log/daemon-script.pid file not found.
...
```
  So, to load the correct log file set  `dir` option in `config/daemons.yml` to my_log_dir_path.

```ruby
  dir_mode: script
  # set a correct log directory path here
  dir: ../../my/log/dir_path
  multiple: false
  backtrace: true
  monitor: true
  ontop: false
```
