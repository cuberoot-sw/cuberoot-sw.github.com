---
layout: post
title: "Shorten URL'S using bit.ly API in Rails"
date: 2013-10-22 10:53
comments: true
author: Prachi
redirect_from: 
  - /2013/10/22/shorten-urls-using-bit-dot-ly-api-in-rails/

---

In this blog we are going to create shorten URL's in rails application using Bit.ly API in ruby step by step.


1. For this you required to have bitly login and api_keys.

2. Create your account on `https://bitly.com` . Get your login and api_key for your account. 

    Check **settings -> Advanced -> show_legacy_key**.
    
3. Steps for creating shorten urls in Ruby using bit.ly API

  - Ruby has a `gem bitly` for bit.ly API. 

<!-- more -->

```ruby
gem install bitly
```
  
  * Configurations

If you want to configure bitly through an initializer. Add code to `config/initializers/bitly.rb`

```ruby
Bitly.configure do |config|
  config.api_version = 3
  config.login = "Bitly_Username"
  config.api_key = "API_KEY"
end
```

To get the client, use Bitly.client:

```ruby
client = Bitly.client
```

You can then use that client to shorten or expand urls :

```ruby
 url = client.shorten('http://cuberoot.in/')
  
 url.long_url
 #=> "http://cuberoot.in/"

 url.short_url
 #=> "http://bit.ly/1eTMCoC"
```

And it is done!
