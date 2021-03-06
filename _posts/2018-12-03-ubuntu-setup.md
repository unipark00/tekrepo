---
title: "Ubuntu 18.04 setup on VirtualBox"
search: false
categories:
  - Ubuntu
last_modified_at: 2019-04-08T10:00+09:00
toc: false
---

This post shows the Ubuntu setup on VirtualBox.  

Used ubuntu image is **18.04.2 (LTS)**.

## 1) Install ssh server on Ubuntu Desktop
```console
# sudo su
[sudo] password for ejungpa:
# apt-get update
# apt-get install openssh-server

// check if port 22 is activated
# netstat -ntl
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address     Foreign Address     State
tcp        0      0 127.0.1.1:53      0.0.0.0:*           LISTEN
tcp        0      0 0.0.0.0:22        0.0.0.0:*           LISTEN
tcp6       0      0 :::22             :::*                LISTEN
tcp6       0      0 :::80             :::*                LISTEN

# /etc/init.d/ssh restart

// if port 22 is NOT activated
# vi /etc/ssh/sshd_config
Port 22
```

## 2) NAT network setup for k8s cluster

A. Create NatNetwork  
   * VirtualBox - File - Preference - Network - NatNetwork (e.g. 10.0.0.0/24)
![NatNetwork1](https://github.com/unipark00/tekrepo/blob/master/_posts/20190411_105355.png?raw=true)  
![NatNetwork2](https://github.com/unipark00/tekrepo/blob/master/_posts/20190411_105409.png?raw=true)

Note) Case of Calico network, 172.16.0.0/24 is preferrable.
[k8s-calico-over-virtualbox-ubuntu-16](
https://blog.encicle.com/2-kubernetes-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-kubernetes-calico-over-virtualbox-ubuntu-16/)

B. Ubuntu Server (Worker node)
```console
root@ubuntu:/etc/network$ vi interfaces

auto lo
iface lo inet loopback

auto enp0s3
iface enp0s3 inet static
address 10.0.0.X
netmask 255.255.255.0
network 10.0.0.0
broadcast 10.0.0.255
gateway 10.0.0.1
```  
![ubuntu-18.04.2](https://github.com/unipark00/tekrepo/blob/master/_posts/20190410_165245.png?raw=true)  
  
C. Ubuntu Desktop (Master node)  
   * Modify configuration via Network Manager instead of interface file.  
   * Settings -> Network -> Ehternet (enp0s3) -> IPv4  
![Desktop1](https://github.com/unipark00/tekrepo/blob/master/_posts/20190411_112303.png?raw=true)  
![Desktop1](https://github.com/unipark00/tekrepo/blob/master/_posts/20190411_112122.png?raw=true)  
