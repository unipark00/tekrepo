---
title: "Creating a single master cluster with kubeadm"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-07T21:30+09:00
toc: true
---

This post shows the creation of a single master cluster with kubeadm.

## Creating a single master cluster with kubeadm

<span style="font-size:20px">`kubeadm`</span> helps you bootstrap a minimum viable Kubernetes cluster that conforms to best practices.

### Installing kubeadm on your hosts
https://github.com/unipark00/k8s/blob/master/cluster/01__install_kubeadm.md 참고

### Initializing the Master node
* Kubernetes 설치 후, **Master Node** 초기화
* Master 노드를 초기화 할 때, 사용할 <span style="color:red">**Pod Network**</span>에 따라 초기화 코드가 달라짐
* 종류 별로 Pod Network 사용 방법 및 초기화 코드 확인  
  https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-netspanwork

The **`master`** is the machine where the control plane components run, including etcd (the cluster database) and the API server (which the kubectl CLI communicates with).  

1. Choose a pod network add-on, and verify whether it requires any arguments to be passed to kubeadm initialization.  
   Depending on which third-party provider you choose, you might need to set the **`--pod-network-cidr`** to a provider-specific value.
   See [Installing a pod network add-on](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-network).  
1. Installing runtime (preparation)  
   ```
   $ kubeadm init --pod-network-cidr=10.244.0.0/16
   ```
1. Unless otherwise specified, kubeadm uses the network interface associated with the default gateway to advertise the master’s IP. To use a different network interface, specify the **`--apiserver-advertise-address=<ip-address>`** argument to kubeadm init.  
1. Run **`kubeadm config images pull`** prior to **`kubeadm init`** to verify connectivity to gcr.io registries.

### Installing a pod network add-on

- You must install a pod network add-on so that your pods can communicate with each other.
- The network must be deployed before any applications.
- CoreDNS will not start up before a network is installed.
- kubeadm only supports **`Container Network Interface (CNI)`** based networks (and does not support kubenet).
- If you find a collision between your network plugin’s preferred Pod network and some of your host networks,  
  you should think of a suitable CIDR replacement and use that during **`kubeadm init`** with **`--pod-network-cidr`**  
  and as a replacement in your network plugin’s YAML.
- You can install a pod network add-on with the following command:  
  ```
  $ kubectl apply -f <add-on.yaml>
  ```
  
  (예) Flannel  
  1) Pass **`--pod-network-cidr=10.244.0.0/16`** to **`kubeadm init`**
  2)  Set **`/proc/sys/net/bridge/bridge-nf-call-iptables`** to **`1`**
  3)  Apply a pod network add-on (Flannel)
  ---
  ```
  $ sysctl net.bridge.bridge-nf-call-iptables=1
  $ sudo kubeadm init --pod-network-cidr=10.244.0.0/16
  $ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/ \
       a70459be0084506e4ec919aa1c114638878db11b/Documentation/kube-flannel.yml
  ```

### Miscellaneous

* [Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
* [Add-ons](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

### Troubleshooting
* Swap disabled
```
$ sed -i '9s/^/Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"\n/' /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
$ systemctl daemon-reload
$ systemctl restart kubelet
```
