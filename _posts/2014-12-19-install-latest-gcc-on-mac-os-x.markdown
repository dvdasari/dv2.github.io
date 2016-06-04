---
layout: post
title:  Install latest version of gcc on Mac OS X
date:   2014-12-19 19:35:03 -0500
comments: true
keywords: gcc
description: Install latest version of gcc on Mac OS X
---

**Mac OS X Yosemite Version 10.10.1**

Updating gcc on Mac OS X can be very tricky. The current version of gcc on my mac was `4.2.1`. I used homebrew to install the latest version `brew install gcc`, which installed `4.9.2`, but checking `gcc —version` still showed `4.2.1`. I followed instructions from a couple of blog posts to get the system point to `4.9.2` but they did not seam to be successful.

```
$ gcc --version
Configured with: —prefix=/Applications/Xcode.app/Contents/Developer/usr —with-gxx-include-dir=/usr/include/c++/4.2.1
Apple LLVM version 6.0 (clang-600.0.56) (based on LLVM 3.5svn)
Target: x86_64-apple-darwin14.0.0
Thread model: posix
```

So I decided to manually install the latest version of gcc from gnu.org

=> Visit: [https://gcc.gnu.org/mirrors.html](https://gcc.gnu.org/mirrors.html) and select a mirror <br/>
=> I picked: [http://mirrors-usa.go-parts.com/gcc/](http://mirrors-usa.go-parts.com/gcc/) <br/>
=> clicked on releases: [http://mirrors-usa.go-parts.com/gcc/releases/](http://mirrors-usa.go-parts.com/gcc/releases/) <br/>
=> selected [http://mirrors-usa.go-parts.com/gcc/releases/gcc-4.9.2/](http://mirrors-usa.go-parts.com/gcc/releases/gcc-4.9.2/) <br/>
=> downloaded gcc-4.9.2.tar.gz [http://mirrors-usa.go-parts.com/gcc/releases/gcc-4.9.2/](http://mirrors-usa.go-parts.com/gcc/releases/gcc-4.9.2/) <br/>

Open iTerm/Terminal and cd into the download and install (took me about 45 minutes)

```
$ cd ~/Downloads/gcc-4.9.2
$ ./configure; make; make install
```
