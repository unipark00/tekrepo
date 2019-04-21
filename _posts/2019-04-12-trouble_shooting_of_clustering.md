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
swapoff -a
```
2. permanent **`swapoff`** after reboot
```console
// based on ubuntu 18.04 LTS
$ vi /etc/fstab
#/swapfile // desktop version
#/swap.img // server version
```
3. kubelet service restart
```console
$ systemctl daemon-reload
$ systemctl restart kubelet
$ sudo journalctl -u kubelet.service | grep "failed to run"
```
4. reset k8s
```console
$ kubeadm reset
```
5. log monitoing with **`journalctl`**
```console
// today log
$ journalctl --since=today
// specific service
$ journalctl -f -u kubelet.service
```
6. `kswapd0` CPU 100% problem
```console
workaround --> 4GB, 4Core VM
```
7. query or create a token (`kubeadm join`)
```console
// Token
$ kubeadm token list
// CA cert hash
$ openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | \
        openssl rsa -pubin -outform der 2>/dev/null | \
        openssl dgst -sha256 -hex | sed 's/^.* //'
or
// New command
$ kubeadm token create --print-join-command
```
