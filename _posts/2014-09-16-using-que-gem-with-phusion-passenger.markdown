---
layout: post
title: "Using Que gem with Phusion Passenger"
date: 2014-09-16 15:23:57 +0530
author: Girish
---

[Que](https://github.com/chanks/que) is a background jobs gem for Ruby and PostgreSQL,
an excellent alternative to DelayedJob if you are using Postgres.

Our [Tweetd](http://www.tweetd.com) project is deployed on a linode slice with nginx and 
[Phusion Passenger](https://www.phusionpassenger.com).

We can run `Que` workers (threads) within the web process. `Que` [documentation](https://github.com/chanks/que/blob/master/docs%2Fadvanced_setup.md)
has usage information about using `unicorn` and `puma` servers, but not `passenger`.

After a bit of googling, came across passenger [docs](https://www.phusionpassenger.com/documentation/Users%20guide%20Nginx.html) which says
`passenger` provides `PhusionPassenger.on_event(:starting_worker_process)` hook to specify our own custom code, when a new process is forked.

Finally, stuffed `application.rb` with following and it worked flawlessly!

```ruby
if defined?(PhusionPassenger)
  PhusionPassenger.on_event(:starting_worker_process) do |forked|
    if forked
      # We're in smart spawning mode.
      ActiveRecord::Base.establish_connection
      Que.mode = :async
    end
  end
end
```
