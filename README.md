# VM for practicing Systems Performance

This repository includes files to create a VM with which a user can try system analytics tools introduced in System Performance (Brendan Gregg 20).

## Overview of the VM

### System

* Ubuntu 20.04
* 2 CPUs
* 4GB Memory
* No GUI

### Tools included in the VM

* perf
* bpftrace
* BPF tools
* sysstat (mpstat, iostat, sar, etc.)
* atop
* hwloc
* valgrind

Note that BPF tools such as `profile` and `execsnoop` will be installed by bpfcc-tools.
Names of the tools installed by bpfcc-tools have postfix (`-bpfcc`).
For example, the command of `profile` is `profile-bpfcc`.
If you want to check a list of BPF tools, see `ls /sbin/*-bpfcc` in the created VM.

### Softwares that consume system resources

Following software is running as sample systems that consume system resource.

* Kubernetes

## Usage

### Prerequisites

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (>=6.1)
* [Vagrant](https://www.vagrantup.com/downloads) (>=2.2)
* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (>=2.10)

### Create a VM

```bash
vagrant up
```

### Login

```bash
vagrant ssh
```

### Exercise

```bash
# Login
vagrant ssh
# Run sample applications
curl -L https://istio.io/downloadIstio | sh -
cd istio-1.9.4
export PATH=$PWD/bin:$PATH
istioctl install --set profile=demo -y
kubectl label namespace default istio-injection=enabled
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
export INGRESS_HOST=$(minikube ip)
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
while true; do curl -sS http://$GATEWAY_URL/productpage > /dev/null ; sleep 3; done
# Open other terminal and try system performance tools
```
