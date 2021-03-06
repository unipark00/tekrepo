---
title: "Installing pod network add-ons"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-10T21:30+09:00
toc: false
---

This post shows the result of installing pod network add-on.

## 1) flannel
```console
ejungpa@k8s-master:~$ kubectl apply -f \
>    https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
podsecuritypolicy.extensions/psp.flannel.unprivileged created
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.extensions/kube-flannel-ds-amd64 created
daemonset.extensions/kube-flannel-ds-arm64 created
daemonset.extensions/kube-flannel-ds-arm created
daemonset.extensions/kube-flannel-ds-ppc64le created
daemonset.extensions/kube-flannel-ds-s390x created

ejungpa@k8s-master:~$ kubectl get nodes
NAME         STATUS   ROLES    AGE   VERSION
k8s-master   Ready    master   23m   v1.14.1

ejungpa@k8s-master:~$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                 READY   STATUS    RESTARTS   AGE
kube-system   coredns-fb8b8dccf-67zqs              1/1     Running   0          24m
kube-system   coredns-fb8b8dccf-hk9kx              1/1     Running   0          24m
kube-system   etcd-k8s-master                      1/1     Running   0          23m
kube-system   kube-apiserver-k8s-master            1/1     Running   0          23m
kube-system   kube-controller-manager-k8s-master   1/1     Running   0          23m
kube-system   kube-flannel-ds-amd64-c9ppn          1/1     Running   0          2m4s
kube-system   kube-proxy-x4tqh                     1/1     Running   0          24m
kube-system   kube-scheduler-k8s-master            1/1     Running   0          23m
```

## 2) Calico
[About Calico](https://docs.projectcalico.org/v3.1/introduction/)
![Calio: How it works](https://docs.projectcalico.org/images/calico-arch-gen-v3.1.svg)

Calico leverages the routing and iptables firewall capabilities native to the Linux kernel. All traffic to and from individual containers, virtual machines, and hosts traverses these in-kernel rules before being routed to its destination.
* `calicoctl`: allows you to achieve advanced policies and networking from a simple, command-line interface.
* `orchestrator plugins`: provide close integration and synchronization with a variety of popular orchestrators.
* `key/value store`: holds Calico’s policy and network configuration state.
* `calico/node`: runs on each host, reads relevant policy and network configuration information from the key/value store, and implements it in the Linux kernel.
