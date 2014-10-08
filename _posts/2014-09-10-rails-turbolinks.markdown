---
layout: post
title: "Rails Turbolinks"
date: 2014-10-08
author: Prachi
---
#### What is turbolinks ?

We know that, in Rails 4 gem `turbolinks` is added by default.
This gem makes our Rails application appears faster to user by using
javascript to replace HTMl body of new pages instead of Full page
reload.

Check in Rails 4 app for turbolinks -

in `gemfile'

```ruby
gem 'turbolinks'
```

in `app/assets/javascripts/application.js`

```js
//= require turbolinks
```
When the `click` event on page fires the request is made by JavaScript and Turbolinks will look at the response’s body. It then uses JavaScript to update the current page with this new content, replacing the title and body elements so that it looks like the new page instead of reloading the page.

#### Issues with javascript functionality
After using turbolinks, it might be possible that some of our existing JavaScript stops working. For example: some click events on page.

When we use Turbolinks this events callback is only triggered on the initial page load.

#### Solutions to fix javascript functionality issues with turbolinks.

There are several events that Turbolinks will trigger, one of which is called page:load. 

```js
  ready = ->
    $('#show_alert').click ->
      alert "Alert- Turbolinks works here."

$(document).ready(ready)
$(document).on('page:load', ready)
```

we set the function on click event to a variable called ready and pass it to both the `document.ready` and the `page:load` events. This way the events for the #show_alert will be attached whether we’ve loaded the page via Turbolinks or not.

Other solution is, we can use `jquery-turbolinks` gem.

Add gem 'jquery-turbolinks' in Gemfile.

```ruby
gem 'jquery-turbolinks'
```
Then in  JavaScript manifest file -

require it immediately after jquery.js. 
Your other scripts should be loaded after jquery.turbolinks.js, and turbolinks.js should be after your other scripts.

```js
//= require jquery
//= require jquery.turbolinks
//= require jquery_ujs
//
// ... your other scripts here ...
//
//= require turbolinks
```
And it  works!

If we create a new Rails 4 application and decide not to use Turbolinks we can easily remove it by commenting-out the gem in the gemfile and the require statement in the application’s JavaScript manifest file.

#### References:
[https://github.com/kossnocorp/jquery.turbolinks](https://github.com/kossnocorp/jquery.turbolinks)
