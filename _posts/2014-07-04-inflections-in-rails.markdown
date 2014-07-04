---
layout: post
title: "Rails ActiveSupport Inflections"
date: 2014-07-04 15:43:05 +0530
author: Prachi
---

We know, In Rails  the `Inflector` transforms words from singular to plural, class names to table names .

The default inflections for pluralization, singularization, and uncountable words are kept in `inflections.rb` .

In this post, we will go through one example explaining the
inflections .

Consider, `leaves_controller` with `Leave model` . 
For this controller routes generated are as follows -

```ruby
new_leafe   GET    /leaves/new(.:format)      leaves#new
edit_leafe  GET    /leaves/:id/edit(.:format) leaves#edit
leafe       GET    /leaves/:id(.:format)      leaves#show

```

```ruby
> 'leaves'.singularize
=> "leafe"
```

here, the singularize format of 'leaves' is taken as 'leafe' .

Suppose, we want to change the singularize format to 'leave', then -

Add rule in `config/initializers/inflections.rb`

```ruby
ActiveSupport::Inflector.inflections do |inflect|
  inflect.irregular 'leave', 'leaves'
end
```

Now, check routes for same -

```ruby
new_leave   GET   /leaves/new(.:format)       leaves#new
edit_leave  GET   /leaves/:id/edit(.:format)  leaves#edit
leave       GET   /leaves/:id(.:format)       leaves#show
```

```ruby
> 'leaves'.singularize
=> "leave"
```

It Works! In this way, by adding `inflection` rules, we can override naming conventions in rails .
