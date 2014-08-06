---
layout: post
title: "Creating Vagrant Box From Existing Instance"
date: 2014-08-06 18:04:02 +0530
author: Girish
---

## Note to self

To create a new box from existing vagrant VirtualBox machine

```
$ vagrant package --base <virtualmachine name> --output ~/PATH/TO/precise32_ruby_187.box
```

look for `<virtualmachine name>` in `~/VirtualBox VMs/` or use following command

```
$ VBoxManage list vms
```

To add a box to vagrant

```
$ vagrant box add precise32_ruby_187 ~/PATH/TO/precise32_ruby_187.box
```

To use the newly created box...

```
$ vagrant init precise32_ruby_187
```
