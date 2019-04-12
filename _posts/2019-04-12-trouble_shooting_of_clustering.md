---
title: "Trouble shooting of k8s cluster with kubeadm"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-12T21:30+09:00
toc: false
---

This post shows the trouble shooting of k8s cluster configuration.

1. swap disabled
```console
$ sed -i '9s/^/Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"\n/' \
    /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
$ systemctl daemon-reload
$ systemctl restart kubelet
```
1. `kubeadm` cheat sheet  
```console
will be updated ...
```
https://kubernetes.io/docs/reference/kubectl/cheatsheet/
1. kubelet service restart
```console
$ systemctl daemon-reload
$ systemctl restart kubelet
$ sudo journalctl -u kubelet.service | grep "failed to run"
```
1. reset k8s
```console
$ kubeadm reset
```
1. **permanent** `swapoff` after reboot
```console
// based on ubuntu 18.04 LTS
$ vi /etc/fstab
# /swapfile // desktop version
# /swap.img // server version
```
1. log monitoing with `journalctl`
```console
// today log
$ journalctl --since=today  
// specific service
$ journalctl -f -u kubelet.service
```
1. `kswapd0` CPU 100% problem
```console
workaround --> 4GB, 4Core VM
```  
1. query or create a token (`kubeadm join`)
```console
$ kubeadm token list
or
$ kubeadm token create --print-join-command
```
