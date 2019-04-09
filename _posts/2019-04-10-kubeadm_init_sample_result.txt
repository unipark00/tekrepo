## kubeadm init result
```console
ejungpa@k8s-master:~$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16
[init] Using Kubernetes version: v1.14.1
[preflight] Running pre-flight checks
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow ternetes.io/docs/setup/cri/
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Activating the kubelet service
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [k8s-master kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svs [10.96.0.1 10.0.0.10]
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [k8s-master localhost] and IPs [10.0.0.10 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [k8s-master localhost] and IPs [10.0.0.10 127.0.0.1 ::1]
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 33.506907 seconds
[upload-config] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.14" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Skipping phase. Please see --experimental-upload-certs
[mark-control-plane] Marking the node k8s-master as control-plane by adding the label "node-role.kubernetes.io/master=''"
[mark-control-plane] Marking the node k8s-master as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: cmroik.cuarzd8oqr80ll1k
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.0.0.10:6443 --token cmroik.cuarzd8oqr80ll1k \
    --discovery-token-ca-cert-hash sha256:0206ab21baffba732884504fc53d800898f04e0e701311b8426afd2023f11203
```

## various monitoring commands
```console
ejungpa@k8s-master:~$ kubectl get nodes
NAME         STATUS     ROLES    AGE     VERSION
k8s-master   NotReady   master   6m29s   v1.14.1

ejungpa@k8s-master:~$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                 READY   STATUS    RESTARTS   AGE
kube-system   coredns-fb8b8dccf-67zqs              0/1     Pending   0          6m16s
kube-system   coredns-fb8b8dccf-hk9kx              0/1     Pending   0          6m16s
kube-system   etcd-k8s-master                      1/1     Running   0          5m47s
kube-system   kube-apiserver-k8s-master            1/1     Running   0          5m27s
kube-system   kube-controller-manager-k8s-master   1/1     Running   0          5m31s
kube-system   kube-proxy-x4tqh                     1/1     Running   0          6m15s
kube-system   kube-scheduler-k8s-master            1/1     Running   0          5m36s

ejungpa@k8s-master:~$ kubectl describe nodes k8s-master
Name:               k8s-master
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=k8s-master
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Tue, 09 Apr 2019 13:55:55 -0400
Taints:             node.kubernetes.io/not-ready:NoExecute
                    node-role.kubernetes.io/master:NoSchedule
                    node.kubernetes.io/not-ready:NoSchedule
Unschedulable:      false
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Tue, 09 Apr 2019 14:03:56 -0400   Tue, 09 Apr 2019 13:55:53 -0400   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Tue, 09 Apr 2019 14:03:56 -0400   Tue, 09 Apr 2019 13:55:53 -0400   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Tue, 09 Apr 2019 14:03:56 -0400   Tue, 09 Apr 2019 13:55:53 -0400   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            False   Tue, 09 Apr 2019 14:03:56 -0400   Tue, 09 Apr 2019 13:55:53 -0400   KubeletNotReady              runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
Addresses:
  InternalIP:  10.0.0.10
  Hostname:    k8s-master
Capacity:
 cpu:                2
 ephemeral-storage:  10253588Ki
 hugepages-2Mi:      0
 memory:             2040928Ki
 pods:               110
Allocatable:
 cpu:                2
 ephemeral-storage:  9449706686
 hugepages-2Mi:      0
 memory:             1938528Ki
 pods:               110
System Info:
 Machine ID:                 40e5d28276384f1c81a5acdd26e8f75e
 System UUID:                e6b63a79-29fa-4b35-a5b8-719a0f13410a
 Boot ID:                    002a132c-5efe-4c9b-813a-da76fe8e28d6
 Kernel Version:             4.18.0-15-generic
 OS Image:                   Ubuntu 18.04.2 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://18.9.4
 Kubelet Version:            v1.14.1
 Kube-Proxy Version:         v1.14.1
PodCIDR:                     10.244.0.0/24
Non-terminated Pods:         (5 in total)
  Namespace                  Name                                  CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                  ------------  ----------  ---------------  -------------  ---
  kube-system                etcd-k8s-master                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         7m22s
  kube-system                kube-apiserver-k8s-master             250m (12%)    0 (0%)      0 (0%)           0 (0%)         7m2s
  kube-system                kube-controller-manager-k8s-master    200m (10%)    0 (0%)      0 (0%)           0 (0%)         7m6s
  kube-system                kube-proxy-x4tqh                      0 (0%)        0 (0%)      0 (0%)           0 (0%)         7m50s
  kube-system                kube-scheduler-k8s-master             100m (5%)     0 (0%)      0 (0%)           0 (0%)         7m11s
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                550m (27%)  0 (0%)
  memory             0 (0%)      0 (0%)
  ephemeral-storage  0 (0%)      0 (0%)
Events:
  Type    Reason                   Age                    From                    Message
  ----    ------                   ----                   ----                    -------
  Normal  NodeHasSufficientMemory  8m40s (x8 over 8m42s)  kubelet, k8s-master     Node k8s-master status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    8m40s (x8 over 8m42s)  kubelet, k8s-master     Node k8s-master status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     8m40s (x7 over 8m42s)  kubelet, k8s-master     Node k8s-master status is now: NodeHasSufficientPID
  Normal  Starting                 7m46s                  kube-proxy, k8s-master  Starting kube-proxy.
```
