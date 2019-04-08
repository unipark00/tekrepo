---
title: "Ubuntu 18.04 setup on VirtualBox"
search: false
categories:
  - Ubuntu
last_modified_at: 2019-04-06T20:30+09:00
toc: true
---

This post shows the miscellaneous Ubuntu setup on VirtualBox.

## 1) Install ssh server on Ubuntu Desktop
```console
# sudo su
[sudo] password for ejungpa:
# apt-get update
# apt-get install openssh-server

// check if port 22 is activated
# netstat -ntl
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 127.0.1.1:53            0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp6       0      0 :::22                   :::*                    LISTEN
tcp6       0      0 :::80                   :::*                    LISTEN

# /etc/init.d/ssh restart

// if port 22 is NOT activated
# vi /etc/ssh/sshd_config
Port 22
```
