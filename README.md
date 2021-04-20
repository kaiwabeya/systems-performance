# VM for practicing Systems Performance

This repository includes files to create a VM with which a user can try system analytics tools that are written in System Performance (Brendan Gregg 20).

## Overview of the VM

### System

* Ubuntu 18.04
* 2 CPUs
* 4GB Memory
* No GUI

### Tools included in the VM

* perf
* BCC tools

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
