---
layout:   post
title:    'Fix for ERROR: Failed to build gem native extension'
date:     '2016-07-05 02:16:00'
category: ruby
tags:     ruby centos error gem gcc yum packages
---

I came across this error when I was installing Jekyll on a CentOS machine. If you are installing a Ruby gem (usually first one on new machine) and you get an error like:

```console
[kausar@centos ~]$ sudo gem install jekyll
Building native extensions. This could take a while...
ERROR: Error installing jekyll:
       ERROR: Failed to build gem native extension.
/usr/bin/ruby extconf.rb
mkmf.rb can't find header files for ruby at /usr/lib/ruby/ruby.h
```

This is usually because Ruby wants to build a gem from source but can't find Ruby development package on the machine. Installing the package `ruby-devel` will fix this issue. The following command shows how to install it on a Centos machine using `yum`.

```bash
sudo yum install ruby-devel
```
