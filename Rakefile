# ignore the "bit" stuff.. only relevant to my custom jekyll fork

desc 'create new post'
# rake new title="New post title goes here"
task :new do
  require 'rubygems'
  title = ENV["title"] || "New Title"
  slug = title.gsub(' ','-').downcase || title.gsub(' ','-').downcase

  TARGET_DIR = "_posts"

  filename = "#{Time.new.strftime('%Y-%m-%d')}-#{slug}.markdown"
  path = File.join(TARGET_DIR, filename)
  post = <<-HTML
---
layout: post
title: "TITLE"
date: DATE
author:
---

  HTML
  post.gsub!('TITLE', title).gsub!('DATE', Time.new.to_s)
  File.open(path, 'w') do |file|
    file.puts post
  end
  puts "new post generated in #{path}"
end
