---
title: "Using Service to Expose App"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-23T21::00+09:00
toc: false
---

This post is about exposing your app publicly.  
[Overview of Kubernetes Services](https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/)  

## Reference
* [kubectl cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* [kubectl run](https://kubernetes.io/docs/reference/kubectl/conventions/#generators) : **`--generator`** flag  

|Resources|kubectl command|
|:--------|:--------------|
|Pod|**`kubectl run --generator=run-pod/v1`**|
|Replication controller|**`kubectl run --generator=run/v1`**|
|Deployment|**`kubectl run --generator=deployment/v1beta1`**|
|Deployment|**`kubectl run --generator=deployment/apps.v1beta1`**|

```console
$ kubectl run hello-world \
        --replicas=5 \
        --labels="run=load-balancer-example" \
        --image=gcr.io/google-samples/node-hello:1.0 \
        --port=8080
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version.
Use kubectl run --generator=run-pod/v1 or kubectl create instead
```
instead,
```console
$ kubectl run hello-world \
        --generator=deployment/v1beta1 \
        --replicas=5 \
        --labels="run=load-balancer-example" \
        --image=gcr.io/google-samples/node-hello:1.0 \
        --port=8080
kubectl run --generator=deployment/v1beta1 is DEPRECATED and will be removed in a future version. 
Use kubectl run --generator=run-pod/v1 or kubectl create instead.
```
## Tips
### Auto completion
```console
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```
## Resource Information
### Nodes
```console
kubectl get no
kubectl get no -o=wide
kubectl get no -o=json // JSON format
```
### Pods
```console
kubectl get pods --all-namespaces
```
## Adding Resources
### Creating Pod
```console
kubectl create -f [name_of_file]
kubectl apply -f [name_of_file]
```
### Running Pod
```console
kubectl run [pod name] --image=[container_image_name] \
        --port=[container port] --labels="<value1>,<value2>" --generator=run-pod/v1
```
