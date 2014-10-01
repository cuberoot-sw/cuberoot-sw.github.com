---
layout: post
title: "Retweet and Reply tweets in rails"
date: 2013-03-01 11:29
comments: true
author: Prachi
categories:
redirect_from: 
  - /2013/03/01/retweet-and-reply-tweets-in-rails/
  - /blog/2013/03/01/retweet-and-reply-tweets-in-rails/

---

I had to add feature for reply and retweet in [www.rubybeats.com](http://rubybeats.com/) site.

Here is how I got it done using the `twitter gem` .

## How to Use :

Add `gem twitter` in your Gemfile .

Run `bundle install` .

Configure Twitter for a twitter authenticated user :

```ruby
  Twitter.configure do |config|
    config.consumer_key = auth_user_CONSUMER_KEY
    config.consumer_secret = auth_user_CONSUMER_SECRET
    config.oauth_token = auth_user_OAUTH_TOKEN
    config.oauth_token_secret = auth_user_OAUTH_TOKEN_SECRET
  end
```

Retweets the specified Tweets as the authenticating user :

```ruby
  Twitter.retweet(tweet_id)

  # Here, tweet_id is an ID of an existing status to which we are retweeting.

  # It will retweets the tweet having id tweet_id as authentication user.

  # It will returns the original tweet with retweet details.
```

Reply the specified Tweet :

```ruby
  reply_text = "here is a replying message"

  Twitter.update( reply_text, "in_reply_to_status_id" => reply_status_id)

  # Use "in_reply_to_status_id" to reply  the existing status that the update is in reply to.

  # Here, the reply_text is  the reply message and, the reply_status_id is an ID of an existing status to which we are replying.

  # It will returns the replied new  Tweet.
```

####References :

  [https://dev.twitter.com/docs/api/1.1/post/statuses/update](https://dev.twitter.com/docs/api/1.1/post/statuses/update)

  [https://dev.twitter.com/docs/api/1.1/post/statuses/retweet/:id](https://dev.twitter.com/docs/api/1.1/post/statuses/retweet/:id)

  [http://rdoc.info/gems/twitter](http://rdoc.info/gems/twitter)
