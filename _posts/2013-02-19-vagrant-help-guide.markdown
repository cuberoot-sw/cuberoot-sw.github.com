---
layout: post
title: "Vagrant Help Guide"
date: 2013-02-19 17:25
comments: true
author: Meghali
categories: 
---
This blog post is about setting up a vagrant box. Vagrants are useful to preserve the app-configurations 
and easy to maintain. Once the vagrant is setup one can create a package out of it; which can be used as 
an independent system with the app-configurations.
Once a package is ready we can use it directly, we don't have to remember the gemset OR rvm etc.
While sharing the APP we can simply share the package file and all others can start using it directly.

## Pre-requisites:
- Install [VirtualBox](http://www.macupdate.com/app/mac/24801/virtualbox)
- Install 'vagrant' gem

### 1. Steps to setup a vagrant box
1. Download one of the vagrant box from the list available at [lucid32.box](http://files.vagrantup.com/lucid32.box)
2. Use that box to setup vagrant for your application 
  `$ vagrant box add blog ~/Downloads/lucid32.box`
3. Initialize the vagrant box added
`$ vagrant init blog`
This will create a Vagrantfile in your project-directory
4. Verify the Vagrantfile 
It should have a line with configuration ` config.vm.box = "blog" ` 
5. Start vagrant
  `$ vagrant up`
6. Connect to vagrant
Once the vagrant is up successfully, try connecting it using ssh command 
  `$ vagrant ssh`
This will connect us to the vagrant shell prompt
7. Usual steps to setup all configuration required for our Application
From this step on, follow the steps to setup a ubuntu machine for “ruby, rails, mysql” etc. environment
8. Create vagrant-package.box
<pre>
  $ vagrant halt (stop the vagrant, before packaging it)
  $ vagrant package
</pre>
It creates default package.box file in the current directory

<!-- more -->
 
### 2. How to use package.box created out of Vagrant
  1) Go to your project directory and run
<pre>
$ vagrant box add my_box /path/to/the/package.box
$ vagrant init my_box
</pre>
  2) Edit the "Vagrantfile" created at project directory, with below line 
    From : #config.vm.forward_port 80, 8080
    To: config.vm.forward_port 3000, 3000
  3) Running it
<pre>
$ vagrant up
$ vagrant ssh
$ cd /vagrant
</pre>
  4) Ready to use
From this step on start using it as usual terminal to start your rails server 
i.e. rails server OR ./script/server

### References:
[RailsCasts Episode-292](http://railscasts.com/episodes/292-virtual-machines-with-vagrant)
