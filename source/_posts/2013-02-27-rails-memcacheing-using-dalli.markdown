---
layout: post
title: "Rails Memcacheing using Dalli"
date: 2013-02-27 15:23
comments: true
categories:
---

Dalli is a High performance memcached client for Ruby.
####Before using dalli -

  1. Dalli requires memcached 1.4+. Check the version using -
          memcached -h

  2. If version is not 1.4+ then install memcached 1.4.x with on mac.
          brew install memcached

  3. You can check memcached is running on your machine or not by command -

          ps -ax | grep memcached
          1455 ??         0:00.90 /usr/local/bin/memcached -d -p 11211
          7629 ttys009    0:00.00 grep memcached

  4. If memcached not running then start it on port 11211.

          /usr/local/bin/memcached -d -p 11211

  5. To Verify installations run

         gem install dalli

  6. Test it with a sample of code

         require 'dalli'
          dc = Dalli::Client.new('localhost:11211')
          dc.set('abc', 123)
          => true
          dc.get('abc')
          => 123

####Using 'Dalli' in your rails 3.X application -
  1. Add ' gem dalli'  in your Gemfile.

  2. Run bundle install

  3. In config/environments/development.rb:

          # Global enable/disable all memcached usage
          config.perform_caching = true

          config.cache_store = :dalli_store, '127.0.0.1'

      set server to localhost use '127.0.0.1'

      For ENV=production in config/environments/production.rb:

          config.cache_store = :dalli_store, your_server

  4. To expire cache in  day

          config.cache_store = :dalli_store, '127.0.0.1', :expires_in => 1.day

  5. Now, you can cache data by using Rails Cache .


