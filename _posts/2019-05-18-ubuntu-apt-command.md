---
title: "apt commands"
search: false
categories:
  - Ubuntu
last_modified_at: 2019-05-18T10:00+09:00
toc: false
---

This post shows various apt commands.

### list installed packages
```console
$ apt list --installed
...
kubeadm/kubernetes-xenial,now 1.14.1-00 amd64 [installed,upgradable to: 1.14.2-00]
kubectl/kubernetes-xenial,now 1.14.1-00 amd64 [installed,upgradable to: 1.14.2-00]
kubelet/kubernetes-xenial,now 1.14.1-00 amd64 [installed,upgradable to: 1.14.2-00]
...
```

### remove installed packages
```console
$ sudo apt remove [pkg_name]  // don't remove configuration files
$ sudo apt purge [pkg_name]   // even remove configuration files
```
