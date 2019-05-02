---
title: "Docker image"
search: false
categories:
  - Docker
last_modified_at: 2019-05-03T10:00+09:00
toc: false
---

This post shows various and useful docker images.

## OS
* [busybox](https://hub.docker.com/_/busybox)  
[BusyBox](https://busybox.net/) combines tiny versions of many common UNIX utilities into a single small executable. It provides replacements for most of the utilities you usually find in GNU fileutils, shellutils, etc. The utilities in BusyBox generally have fewer options than their full-featured GNU cousins; however, the options that are included provide the expected functionality and behave very much like their GNU counterparts. BusyBox provides a fairly complete environment for any small or embedded system.  
* [alpine](https://hub.docker.com/_/alpine/)  
[Alpine Linux](https://alpinelinux.org/) is a Linux distribution built around musl libc and BusyBox. The image is only 5 MB in size and has access to a package repository that is much more complete than other BusyBox based images. This makes Alpine Linux a great image base for utilities and even production applications. Read more about Alpine Linux here and you can see how their mantra fits in right at home with Docker images.
