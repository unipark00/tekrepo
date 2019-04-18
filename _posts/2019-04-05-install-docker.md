---
title: "Install docker as preparation for k8s cluster setup"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-04-06T20:30+09:00
toc: false
---

This post shows the Installation of docker as preparation for Kubernetes cluster setup.

## CRI installation
Since v1.6.0, Kubernetes has enabled the use of **`CRI`**, Container Runtime Interface, by default.

Since v1.14.0, kubeadm will try to automatically detect the container runtime on Linux nodes by scanning through a list of well known domain sockets.

## Install docker (All Nodes)
```console
# Install Docker CE
## Set up the repository
### Install packages to allow apt to use a repository over HTTPS
sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates \
    curl gnupg-agent software-properties-common

### Add Docker’s official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

### Add Docker apt repository.
sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable"

## Install Docker CE
sudo apt-get update && sudo apt-get install -y docker-ce=18.06.2~ce~3-0~ubuntu
-----
# Setup daemon
sudo su
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

mkdir -p /etc/systemd/system/docker.service.d
-----
# Restart docker
systemctl daemon-reload
systemctl restart docker
```
[https://kubernetes.io/docs/setup/cri/#docker](https://kubernetes.io/docs/setup/cri/#docker)

## Post-installation steps for Linux
```console
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ docker run hello-world
```
