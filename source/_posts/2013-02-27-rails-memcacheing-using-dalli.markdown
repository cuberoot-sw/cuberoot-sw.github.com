---
layout: post
title: "Rails Memcacheing using Dalli"
date: 2013-02-27 15:23
comments: true
author: Prachi
categories:
---

Dalli is a High performance memcached client for Ruby.
####Before using dalli -

Dalli requires memcached 1.4+. Check the version using -
```
  $ memcached -h
```
  `memcached -h` shows memcached version and help .

If version is not 1.4+ then install memcached 1.4.x with on mac.
```
  $ brew install memcached
```

You can check memcached is running on your machine or not by command -
```
  $ ps -ax | grep memcached
    1455 ??         0:00.90 /usr/local/bin/memcached -d -p 11211
    7629 ttys009    0:00.00 grep memcached
```

If memcached not running then start it on port 11211.
```
  $ /usr/local/bin/memcached -d -p 11211
```

To Verify installations run
```
  $ gem install dalli
```

Test it with a sample of code
```ruby
         require 'dalli'
          dc = Dalli::Client.new('localhost:11211')
          dc.set('abc', 123)
          => true
          dc.get('abc')
          => 123
```

####Using 'Dalli' in your rails 3.X application -
Add `gem dalli`  in your Gemfile.

Run `bundle install`

In `config/environments/development.rb` :
```ruby
  # Global enable/disable all memcached usage
  config.perform_caching = true

  config.cache_store = :dalli_store, '127.0.0.1'
```
  set server to localhost use `127.0.0.1`

  For ENV=production in `config/environments/production.rb`:
```ruby
  config.cache_store = :dalli_store, your_server
```

To expire cache in  day
```ruby
  config.cache_store = :dalli_store, '127.0.0.1', :expires_in => 1.day
```

Now, you can cache data by using Rails Cache .


