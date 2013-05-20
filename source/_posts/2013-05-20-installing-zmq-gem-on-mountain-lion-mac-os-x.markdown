---
layout: post
title: "Installing zmq gem on Mountain Lion Mac OS X"
date: 2013-05-20 15:55
comments: true
categories: 
author: Girish
---

I decided to play with [ZeroMQ](http://www.zeromq.org) on my Mac. Had to spend about an hour installing the `zmq` gem, hope this post saves you the trouble.

## Install `ZeroMQ`
First I tried to install `ZeroMQ` using brew.
```
$ brew install zeromq
```
It installed `ZeroMQ` version 3.2.2. Gem install `zmq` failed with
following error...

```
$ ARCHFLAGS="-arch x86_64" gem install zmq -- --with-zmq-dir=/usr/local
Building native extensions.  This could take a while...
ERROR:  Error installing zmq:
	ERROR: Failed to build gem native extension.

        /Users/girish/.rvm/rubies/ruby-1.9.3-p327/bin/ruby extconf.rb --with-zmq-dir=/usr/local
checking for zmq.h... yes
checking for zmq_init() in -lzmq... yes
Cool, I found your zmq install...
creating Makefile

make
compiling rbzmq.c
rbzmq.c:968:7: error: use of undeclared identifier 'ZMQ_RECOVERY_IVL_MSEC'
        case ZMQ_RECOVERY_IVL_MSEC:
             ^
rbzmq.c:990:10: error: use of undeclared identifier 'ZMQ_HWM'
    case ZMQ_HWM:
         ^
rbzmq.c:991:10: error: use of undeclared identifier 'ZMQ_SWAP'
    case ZMQ_SWAP:
         ^
...
```

After googling for some time, it turned out that the `zmq` gem is not updated for the latest
stable version 3.2.2 of `ZeroMQ`, it works (basically installs) with
version 2.2.0.

## Install `ZeroMQ` 2.2.0 from sources

```bash
$ wget http://download.zeromq.org/zeromq-2.2.0.tar.gz
$ tar xfz zeromq-2.2.0.tar.gz
$ cd zeromq-2.2.0
$ ./configure
$ make
$ sudo make install
```

<!-- more -->

## Install `zmq` gem
Finally install the `zmq` gem

```
$ ARCHFLAGS="-arch x86_64" gem install zmq  -- --with-zmq-dir=/usr/local --with-zmq-lib=/usr/local/lib and --with-zmq-include=/usr/local/include
Building native extensions.  This could take a while...
Successfully installed zmq-2.1.4
1 gem installed
```
And bravo it worked!
