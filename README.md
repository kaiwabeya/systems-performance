# VM for practicing Systems Performance

This repository includes files to set up system monitoring tools that are introduced in System Performance (Brendan Gregg 20).

The two options are available.

* [On premise]: System monitoring tools will be installed to your machine
* [VM]: A new VM that has system monitoring tools will be created

## On premise

### Prerequisite

* Ubuntu 20.04
* Python3
* Ansible

### Setup

```bash
cd provisioning
ansible-galaxy install geerlingguy.docker -p provisioning/roles
ansible-galaxy install geerlingguy.kubernetes -p provisioning/roles
ansible-playboot -i hosts site.yml
```

## VM

### Overview of created VM

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

A Kubernetes single cluster is running in the created VM as a sample system that consumes system resource.

In addition, sample applications (Istio and [Bookinfo sample application](https://istio.io/latest/docs/examples/bookinfo/)) can start by `launch.sh` that is included in the VM.
By requesting repeatedly, you can put workload so that you can enjoy system performance tools.

## Usage

### Prerequisites

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (6.1.18)
* [Vagrant](https://www.vagrantup.com/downloads) (>=2.2)
* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (>=2.10)

Note that the latest VirtualBox may not have compatibility to the latest Vagrant.

### Create a VM

```bash
ansible-galaxy install geerlingguy.docker -p provisioning/roles
ansible-galaxy install geerlingguy.kubernetes -p provisioning/roles
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
./launch.sh

# Put workload
while true; do curl -sS http://$GATEWAY_URL/productpage > /dev/null ; sleep 0.1; done

# Open other terminal and try system performance tools
```

### Tear down

```bash
vagrant destroy -f
```
