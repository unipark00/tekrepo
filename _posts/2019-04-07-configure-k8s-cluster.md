---
title: "Creating a single master cluster with kubeadm"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-07T21:30+09:00
toc: false
---

This post shows the creation of a single master cluster with kubeadm.

[https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/)  

# Overview
* Kubernetes 설치 후, **`Master Node`** 초기화
* Master 노드를 초기화 할 때, 사용할 **`Pod Network`**에 따라 초기화 코드가 달라짐
* 종류 별로 Pod Network 사용 방법 및 초기화 코드 확인  
  [https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-netspanwork](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-netspanwork)  


# Instructions

## Installing kubeadm on your hosts
**`kubeadm`** helps you bootstrap a minimum viable Kubernetes cluster that conforms to best practices.
[https://unipark00.github.io/tekrepo/kubernetes/install-kubeadm/](https://unipark00.github.io/tekrepo/kubernetes/install-kubeadm/)

## Initializing your master
The **`master`** is the machine where the control plane components run, including etcd (the cluster database) and the API server (which the kubectl CLI communicates with).  
1. Choose a pod network add-on, and verify whether it requires any arguments to be passed to kubeadm initialization. Depending on which third-party provider you choose, you might need to set the **`--pod-network-cidr`** to a provider-specific value. See [Installing a pod network add-on](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-network).  
1. (Optional) To use different container runtime or if there are more than one installed on the provisioned node, specify the **`--cri-socket`** argument to **```kubeadm init```**.
1. (Optional) Unless otherwise specified, kubeadm uses the network interface associated with the default gateway to advertise the master’s IP. To use a different network interface, specify the **`--apiserver-advertise-address=<ip-address>`** argument to **`kubeadm init`**.  
1. (Optional) Run **`kubeadm config images pull`** prior to **`kubeadm init`** to verify connectivity to gcr.io registries.

To make kubectl work for your non-root user, run these commands, which are also part of the **`kubeadm init`** output:
```console
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Warning !!!
[ERROR Swap]: running with swap on is not supported. Please disable swap [#610](https://github.com/kubernetes/kubeadm/issues/610)
```
$ swapoff -a // 이것 때문에 엄청 삽질을~~!!!
```

## Installing a pod network add-on
* You must install a pod network add-on so that your pods can communicate with each other.
* The network must be deployed before any applications.
* CoreDNS will not start up before a network is installed.
* kubeadm only supports **`Container Network Interface (CNI)`** based networks (and does not support kubenet).
* If you find a collision between your network plugin’s preferred Pod network and some of your host networks,  
  you should think of a suitable CIDR replacement and use that during **`kubeadm init`** with **`--pod-network-cidr`**  
  and as a replacement in your network plugin’s YAML.

You can install a pod network add-on with the following command:  
```console
kubectl apply -f <add-on.yaml>
```

### Flannel  
1. Set **`/proc/sys/net/bridge/bridge-nf-call-iptables`** to **`1`**  
2. Pass **`--pod-network-cidr=10.244.0.0/16`** to **`kubeadm init`**  
```console
$ sudo sysctl net.bridge.bridge-nf-call-iptables=1
$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```
Note) [kubeadm init result](https://github.com/unipark00/tekrepo/blob/master/_posts/kubeadm_init.txt)  
3. Apply a pod network add-on (Flannel)  
```console
$ kubectl apply -f \
      https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
podsecuritypolicy.extensions/psp.flannel.unprivileged created
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.extensions/kube-flannel-ds-amd64 created
daemonset.extensions/kube-flannel-ds-arm64 created
daemonset.extensions/kube-flannel-ds-arm created
daemonset.extensions/kube-flannel-ds-ppc64le created
daemonset.extensions/kube-flannel-ds-s390x create
```
4. Check that the CoreDNS pod is Running
```console
$ kubectl get nodes
$ kubectl get pods --all-namespaces
$ kubectl describe nodes k8s-master
```
5. Etc
```
$ kubeadm reset
$ systemctl daemon-reload
$ systemctl restart kubelet
$ sudo journalctl -u kubelet.service | grep "failed to run"
```

# Miscellaneous
* [Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
* [Add-ons](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

# Troubleshooting
* Swap disabled
```console
$ sed -i '9s/^/Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"\n/' \
    /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
$ systemctl daemon-reload
$ systemctl restart kubelet
```
