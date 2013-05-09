---
layout: post
title: "Multilingual Rails Application"
date: 2013-05-09 15:47
comments: true
author: Prachi
categories:
---

#### In this blog we are going through make aur simple rails blog application multilingual that is to support it with multiple language.

In rails application, i want to make it multilingual. But we need to define YAML language files for all required languages and tell the Rails application which language it should currently use.

Rails `I18n.locale` saves the current language and can be read by the application.

#### Steps for language translation

Lets we use German and English language here for translation .
#### 1. Add language.yml in config/locales

In `/config/locales/en.yml`
```ruby
  en:
    hello: "Hello World"
```

In `/config/locales/de.yml`
```ruby
  hi:
    hello: "hallo Welt"
```
#### 2. Configuration changes
In rails application the default locale is :en for English.
Add in `config/application.rb`
```ruby
  config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}')]
```

#### 3. Setting and Passing the Locale

Setting a selected locale as current language is easy. You can set the locale in the `ApplicationController` using before_filter

```ruby
  before_filter :set_locale

  def set_locale
    I18n.locale = params[:locale] if params[:locale].present?
  end
```

#### 4.  Adding Translations
Then we insert the links for the navigation in the `app/views/layouts/application.html.erb`:
```ruby
  <p>
    <%= link_to_unless I18n.locale == :en, "English", locale: :en %>
    |
    <%= link_to_unless I18n.locale == :de, "Deutsch", locale: :de %>
  </p>
```

The I18n API's  `translate` method is used to translate the text into current language.

So, change the view to  display translated text. Use I18n API's `#t` helper to translated string as

e.g in posts#index
```ruby
<h1><%=t :hello %></h1>
```

Now, when you run posts#index the heading will display translated string for hello as per language.
You can test translation by switching languages.

#### 5. Organization of Locale Files

Putting translations for all parts of an application in one file per locale could be hard to manage.so we  can store these files in a hierarchy as follows.
```ruby
|-defaults
|---de.rb
|---en.rb
|-models
|---post
|-----de.rb
|-----en.rb
|-views
|---defaults
|-----de.rb
|-----en.rb
|---posts
|-----de.rb
|-----en.rb
```









