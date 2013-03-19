---
layout: post
title: "'create_yetting_class: undefined method 'each' for nil:NilClass"
date: 2013-03-12 16:59
comments: true
author: Prachi
categories:
---

#### In this blog we are going through some issues which I faced while upgrading `gem twitter` and their fixes while deploying rails app on production.

* Running a ruby script xxtweetstream.rb using `Yettings` in Rails `production environment` .

  while running a xxtweetstream.rb using Yettings in production with command :

```ruby
$ bundle exec ruby xxtweetstream.rb
```

  But following error occured :
```ruby
/var/www/iplbeats/shared/bundle/ruby/1.9.1/gems/yettings-0.1.1/lib/yettings.rb:27:in `create_yetting_class': undefined method `each' for nil:NilClass (NoMethodError)
	from /var/www/iplbeats/shared/bundle/ruby/1.9.1/gems/yettings-0.1.1/lib/yettings.rb:11:in `block in setup!'
	from /var/www/iplbeats/shared/bundle/ruby/1.9.1/gems/yettings-0.1.1/lib/yettings.rb:10:in `each'
	from /var/www/iplbeats/shared/bundle/ruby/1.9.1/gems/yettings-0.1.1/lib/yettings.rb:10:in `setup!'
	from /var/www/iplbeats/shared/bundle/ruby/1.9.1/gems/yettings-0.1.1/lib/yettings/railtie.rb:7:in `block in <class:Railtie>'
	from /var/www/iplbeats/shared/bundle/ruby/1.9.1/gems/railties-3.2.11/lib/rails/initializable.rb:30:in `instance_exec'
```

  Run xxtweetstream.rb in production enviornment with specifying `RAILS_ENV=production` .

```ruby
$ RAILS_ENV=production bundle exec ruby xxtweetstream.rb
```

<!-- more -->

* While running script with Yettings, an error occurs for `config/enviornment` .

```ruby
  xxtweetstream.rb:11:in `require': cannot load such file -- /Users/prachi/work/cuberoot/rubybeats/aggregator/config/environment.rb (LoadError)
  from xxtweetstream.rb:11:in `<main>'
```


  To fix this error, added following code with using `File.join` in `xxtweetstream.rb` file .

```ruby
  root = File.expand_path(File.dirname(__FILE__))

  # added following code to require following files with correct path
  require File.join(root + '/..', "config", "environment")
```

* Running daemon on rails production env by using `gem daemons-rails` .

  The file `config/daemons.yml` is added by installing `gem daemons-rails` .

  When I started a daemon  by running :
```ruby
$ bundle exec ruby lib/daemons/xxtweetstream_ctl start
```

  Got an error like :
```ruby
...
releases/log/xxtweetstream.rb.pid file not found.
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
