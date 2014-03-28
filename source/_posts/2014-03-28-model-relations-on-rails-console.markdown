---
layout: post
title: "Model Relations on Rails Console"
date: 2014-03-28 13:04
comments: true
categories:
author: Prachi
---

We know that rails console is useful for testing ruby syntax and model queries. Also it is useful for changing server-side data without touching the website.

On Rails console, we can view Model Associations.

There is a simple method `reflect_on_all_associations`, by using which we
get the model associations array on console.

```ruby
irb(main):001:0> Patient.reflect_on_all_associations.each do |a|
irb(main):002:1* puts "#{a.macro} => #{a.name}"
irb(main):003:1> end

has_many => tsh_reports
has_many => hbs_ag_reports

=> [#<ActiveRecord::Reflection::AssociationReflection:0x007fd4b4209a78 @macro=:has_many, @name=:tsh_reports, @scope=nil, @options={}, @active_record=Patient(no database connection), @plural_name="tsh_reports", @collection=true>, #<ActiveRecord::Reflection::AssociationReflection:0x007fd4b4bfdd90 @macro=:has_many, @name=:hbs_ag_reports, @scope=nil, @options={}, @active_record=Patient(no database connection), @plural_name="hbs_ag_reports", @collection=true>]
```
That's it! We have done.
