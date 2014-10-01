---
layout: post
title: "Fixing corrupt vagrant"
date: 2013-03-12 12:19
comments: true
author: Meghali Dhoble
categories: 
redirect_from: 
  - /2013/03/12/fixing-corrupt-vagrant/
  - /blog/2013/03/12/fixing-corrupt-vagrant/

---

Recently my vagrant machine crashed. This is how I used 'fsck' to recover it.

1. Power off the vagrant machine using
  `vagrant halt`
2. Before booting it again enable 'gui' mode with un-commenting the below line from "Vagrantfile"
  `config.vm.boot_mode = :gui`
3. Now run `vagrant up`; it would open an gui-interface with an shell terminal open for vagrant-machine 
4. Login to shell (started up during the previous step) with 'username/password' as 'vagrant/vagrant'
5. Once logged-in, go to root-dir
    `cd /`
6. Create a file named 'forcefsck' at root-dir
    `sudo touch /forcefsck`
7. Reboot the system

Now the vagrant should be up and running without any problem.
