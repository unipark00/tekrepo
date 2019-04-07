---
title: "Install docker as preparation for k8s cluster setup"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-07T20:49:51+09:00
toc: true
---

This post shows the Installation of docker as preparation for Kubernetes cluster setup.

## CRI installation
Since v1.6.0, Kubernetes has enabled the use of `CRI`, Container Runtime Interface, by default.

Since v1.14.0, kubeadm will try to automatically detect the container runtime on Linux nodes by scanning through a list of well known domain sockets.

### Install docker (All Nodes)
```console
# sudo apt-get update
# sudo apt-get install apt-transport-https ca-certificates \
    curl gnupg-agent software-properties-common
# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo apt-key add -
# sudo add-apt-repository "deb [arch=amd64] \
    https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable"
# sudo apt-get update && sudo apt-cache search docker-ce
# sudo apt-get install docker-ce docker-ce-cli containerd.io
```
[https://kubernetes.io/docs/setup/cri/#docker](https://kubernetes.io/docs/setup/cri/#docker)
