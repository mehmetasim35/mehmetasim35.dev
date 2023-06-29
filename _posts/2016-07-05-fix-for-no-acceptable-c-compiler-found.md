---
layout:   post
title:    'Fix for error: no acceptable C compiler found in $PATH'
date:     '2016-07-05 02:51:00'
category: installation
tags:     ruby centos error gem make
---

While installing Jekyll on a new CentOS machine I got this this error.

```console
configure: error: in `/usr/lib/ruby/gems/1.8/gems/ffi-1.9.10/ext/ffi_c/libffi-x86_64-linux':
configure: error: no acceptable C compiler found in $PATH
See `config.log' for more details
make: *** ["/usr/lib/ruby/gems/1.8/gems/ffi-1.9.10/ext/ffi_c/libffi-x86_64-linux"/.libs/libffi_convenience.a] Error 1
```

This usually happends when Ruby wants to build a gem (usually first one) from source and it requires a C compiler. This can also happen when you try to compile a piece of software from source and that software is written in C. Usually you will have some sort of `Makefile` and will ask you to run `make install` etc. This is when the error can occur.

To fix this all you need is to install a C compiler. GNU Compiler Collection includes compiler for C. The package can be installed using the following command on a CentOS machine utilising `yum`.

```bash
sudo yum install gcc
```
