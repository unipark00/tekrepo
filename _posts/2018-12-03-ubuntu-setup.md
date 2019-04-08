---
title: "Ubuntu 18.04 setup on VirtualBox"
search: false
categories:
  - Ubuntu
last_modified_at: 2019-04-08T10:00+09:00
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

## 2) Kubernetes Cluster를 위한 NAT Network 설정하기

1. NatNetwork 만들기
* VirtualBox - File - Preference - Network - NatNetwork (10.0.0.0/24)

2. Ubuntu Server (k8s worker) 설정
```console
root@ubuntu:/etc/network$ vi interfaces

# 다음과 같이 설정하면 된다.
auto enp0s3
iface enp0s3 inet dhcp

auto enp0s3
iface enp0s3 inet static
address 10.0.0.X
netmask 255.255.255.0
network 10.0.0.1
broadcast 10.0.0.255
```

3. Ubuntu Desktop 설정
interface 파일 대신 Network Manager로 수정

* Settings -> Network -> Ehternet (enp0s3) -> Wired -> IPv4
  * IPv4 Method를 Manual로 변경 후, 해당 정보 입력
  * DNS는 AUTO
  