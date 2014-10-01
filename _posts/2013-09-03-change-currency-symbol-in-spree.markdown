---
layout: post
title: "Change currency symbol in spree"
date: 2013-09-03 15:53
comments: true
author: Prachi
categories: 
redirect_from: 
  - /2013/09/03/change-currency-symbol-in-spree/

---

I am using spree 1.3.3 in my rails application with INR currency and want to convert its symbol to '&#8377;'

In spree, `money` gem is used for money and currency conversion which takes old symbol 'Rs' for INR currency.

As in spree there is no default configuration to set currency_symbol, we have to provide `:symbol` option which will be set by money gem. 

To do this , I have override `initialize` method of spree's `money class` .


First you have to check that your application is able to load `../lib/**/*_decorator.rb` . If not, please configured your rails app to load it.

<!-- more -->

In `config/application.rb` add -

```ruby
# Load Lib decorators

Dir.glob(File.join(File.dirname(__FILE__), "../lib/**/*_decorator*.rb")) do |c|
  Rails.configuration.cache_classes ? require(c) : load(c)
end
```


In `/lib/spree/money_decorator.rb`

```ruby

   def initialize(amount, options={})
   	 @money = ::Money.parse([amount, (options[:currency] || Spree::Config[:currency])].join)
    	@options = {}
    	@options[:with_currency] = Spree::Config[:display_currency]
    	@options[:symbol_position] = Spree::Config[:currency_symbol_position].to_sym
    	@options[:no_cents] = Spree::Config[:hide_cents]
    	@options[:decimal_mark] = Spree::Config[:currency_decimal_mark]
    	@options[:thousands_separator] = Spree::Config[:currency_thousands_separator]
    	
	    # add currency_symbol
    	@options[:symbol] = '&#8377;'
    
	    @options.merge!(options)
    	# Must be a symbol because the Money gem doesn't do the conversion
    	@options[:symbol_position] = @options[:symbol_position].to_sym
  end

```


Now it works, the money gem takes care of :symbol and use it to convert to currency with our custom_symbol '&#8377;'
