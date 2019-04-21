---
title: "Kubernetes Cheat Sheet"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-18T21::00+09:00
toc: false
---

This post shows the cheat sheets for quick k8s operation.

## Reference
* [kubectl cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Tips
### Auto completion
```console
$ source <(kubectl completion bash)
$ echo "source <(kubectl completion bash)" >> ~/.bashrc
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
