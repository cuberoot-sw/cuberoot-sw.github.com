---
layout: post
title: "store JSON object in database"
date: 2014-03-07 12:01
comments: true
categories: 
author: Prachi
---

In my rails application, I want to store JSON/Array object in database.

We can do it, by using follwing simple steps:

Define your field as a string (or text) in the migration and then:

```ruby
  class Message < ActiveRecord::Base
    serialize :attachment, JSON # hash object
    serialize :channels, JSON # array object
  end
```

Then, you can save attachment and  as JSON object as:
```ruby
  message = Message.new
  message.attachment = {'name' => 'File1', 'url' => 'path/to/attachment'}
  message.channels = ['abc', 'xyz']
  message.save
```

That's it. You done with saving JSON object in your database.
