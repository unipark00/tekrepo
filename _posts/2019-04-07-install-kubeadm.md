---
title: "Installing kubeadm, kubelet and kubectl" 
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-07T20:39:00+09:00
toc: true
---

This post shows the Installation of kubeadm, kubelet and kubectl.

## Installing kubeadm, kubelet and kubectl

### Master node : kubelet, kubeadm, kubectl
```console
# sudo apt-get update && sudo apt-get install -y apt-transport-https curl
# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
# sudo echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | \
    sudo tee -a /etc/apt/sources.list.d/kubernetes.list
# sudo apt-get update && sudo apt-get install -y kubelet kubeadm kubectl
# sudo apt-mark hold kubelet kubeadm kubectl
```

### Worker node(s) : kubelet, kubeadm
```console
# sudo apt-get update && sudo apt-get install -y apt-transport-https curl
# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
# sudo echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | \
    sudo tee -a /etc/apt/sources.list.d/kubernetes.list
# sudo apt-get update && sudo apt-get install -y kubelet kubeadm
# sudo apt-mark hold kubelet kubeadm
```

The `kubelet` is the node agent that will run all the pods for us, including the kube-system pods.  
The `kubeadm` is a tool for deploying multi-node kubernetes clusters.  
The `kubectl` is the command line tool for interacting with Kubernetes.  

We’ve installed specific versions and `marked` them to hold so that Kubernetes and Docker don’t automatically update and become incompatible.  


## Overview of kubeadm
`kubeadm` is the command line tool for easily provisioning a secure Kubernetes cluster on top of physical or cloud servers or virtual machines (currently in alpha).  

[https://kubernetes.io/docs/setup/cri/#dockerhttps://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/#what-s-next](https://kubernetes.io/docs/setup/cri/#dockerhttps://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/#what-s-next)
