---
title: "Creating a single master cluster with kubeadm"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-07T21:30+09:00
toc: false
---

This post shows the creation of a single master cluster with kubeadm.  

# Overview
* Kubernetes 설치 후, **`Master Node`** 초기화
* Master 노드를 초기화 할 때, 사용할 **`Pod Network`** 에 따라 초기화 코드가 달라짐
* 종류 별로 Pod Network 사용 방법 및 초기화 코드 확인  
  [https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-netspanwork](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-netspanwork)  

## 1) Installing kubeadm on your hosts
**`kubeadm`** helps you bootstrap a minimum viable Kubernetes cluster that conforms to best practices.
[https://unipark00.github.io/tekrepo/kubernetes/install-kubeadm/](https://unipark00.github.io/tekrepo/kubernetes/install-kubeadm/)

## 2) Initializing your master node
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
### Warning !!!
[ERROR Swap]: running with swap on is not supported. Please disable swap [#610](https://github.com/kubernetes/kubeadm/issues/610)
```
sudo swapoff -a // 이것 때문에 엄청 삽질을~~!!!
```
## 3) Installing a pod network add-on
* You must install a pod network add-on so that your pods can communicate with each other. ([add-on pages](https://kubernetes.io/docs/concepts/cluster-administration/addons/))  
* The network must be deployed before any applications. CoreDNS will not start up before a network is installed.  
* kubeadm only supports **`Container Network Interface (CNI)`** based networks (and does not support kubenet).  
* If you find a collision between your network plugin’s preferred Pod network and some of your host networks,  
  you should think of a suitable CIDR replacement and use that during **`kubeadm init`** with **`--pod-network-cidr`**  
  and as a replacement in your network plugin’s YAML.

You can install a pod network add-on with the following command:  
```console
kubectl apply -f <add-on.yaml>
```  
### Flannel
![flannel](https://github.com/unipark00/tekrepo/blob/master/_posts/20190411_132750.png?raw=true)  
1. Set **`/proc/sys/net/bridge/bridge-nf-call-iptables`** to **`1`**  
2. Pass **`--pod-network-cidr=10.244.0.0/16`** to **`kubeadm init`**  
```console
sudo sysctl net.bridge.bridge-nf-call-iptables=1
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```
3. Install Flannel with the following command.
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
kubectl get nodes
kubectl get pods --all-namespaces
kubectl get pods -n kube-system
kubectl describe nodes k8s-master
```  

### Calico
![calico](https://github.com/unipark00/tekrepo/blob/master/_posts/20190418_182218.png?raw=true)
1. Set **`/proc/sys/net/bridge/bridge-nf-call-iptables`** to **`1`**  
2. Pass **`--pod-network-cidr=192.168.0.0/16`** to **`kubeadm init`**  
```console
sudo sysctl net.bridge.bridge-nf-call-iptables=1
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```
3. Install Flannel with the following command.
```console
$ kubectl apply -f \
    https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml
configmap/calico-config created
service/calico-typha created
deployment.apps/calico-typha created
poddisruptionbudget.policy/calico-typha created
daemonset.extensions/calico-node created
serviceaccount/calico-node created
customresourcedefinition.apiextensions.k8s.io/felixconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgppeers.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgpconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ippools.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/hostendpoints.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/clusterinformations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworksets.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networkpolicies.crd.projectcalico.org created
      
```

## 4) Installing a dashboard
https://github.com/kubernetes/dashboard
* Getting Started
```console
$ kubectl create -f \
      https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```  
* Create An Authentication Token (RBAC)
```console
// w/o service account
kubectl -n kube-system describe $(kubectl -n kube-system \
      get secret -n kube-system -o name | grep namespace) | grep token
or
// w/ service account (안됨~~)
kubectl create serviceaccount cluster-admin-dashboard-sa
kubectl create clusterrolebinding cluster-admin-dashboard-sa \
      --clusterrole=cluster-admin --serviceaccount=default:cluster-admin-dashboard-sa
kubectl get secret $(kubectl get serviceaccount cluster-admin-dashboard-sa \
      -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode
```

# Miscellaneous
* [Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
* [Add-ons](https://kubernetes.io/docs/concepts/cluster-administration/addons/)
* [What is CNI?](https://github.com/containernetworking/cni#cni---the-container-network-interface)
* [Monitoring Kubernetes with Prometheus](https://linuxacademy.com/blog/kubernetes/running-prometheus-on-kubernetes/?utm_source=website&utm_medium=blog&utm_campaign=2019_kubernetes)
