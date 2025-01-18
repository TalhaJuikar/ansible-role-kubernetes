Ansible Role: Kubernetes.multi
=========
[![Push to Ansible Galaxy](https://github.com/TalhaJuikar/ansible-role-kubernetes/actions/workflows/publish.yml/badge.svg)](https://github.com/TalhaJuikar/ansible-role-kubernetes/actions/workflows/publish.yml)

Ansible role to install and configure kubernetes cluster on 
RedHat/Debian based systems.
Creates a multi-node cluster with one master and multiple worker nodes. Also, optionally installs metallb for load balancing.

This role is tested on the following OS:
- Rocky Linux 9
- CentOS 9 Stream
- Ubuntu 22.04 / 24.04 
- Debian 11 / 12


Role Variables
--------------
The following variables are used in this role:
The default values are set in `defaults/main.yml`.
```bash
copy_kubeconfig: # Default is true. Set to false if you don't want to copy kubeconfig to your local machine.
kubernetes_version: "" # Default is 1.31
crio_version: "" # Default is 1.31
container_runtime: "" # Can be either containerd or crio. Default is crio. 
kubernetes_pod_network:
  cni:
  cidr: 
  # Default is calico 192.168.0.0/16
  # Other options are flannel 10.244.0.0/16
metallb_ip_range: ""
```
You can override these variables in your playbook.

```yaml
---
- name: Install kubernetes
  hosts: all
  become: true
  roles:
    - TalhaJuikar.kubernetes
  vars:
    container_runtime: "containerd"
```


Example Playbook
----------------

```yaml
---
- name: Install kubernetes
  hosts: all
  become: true
  roles:
    - TalhaJuikar.kubernetes
  vars:
    kubernetes_version: "1.30"
    container_runtime: "containerd"
    kubernetes_pod_network:
      cni: 'flannel'
      cidr: '10.244.0.0/16'
```

Example Inventory
----------------

```ini
[k8s_master]
host1

[k8s_worker]
worker1
worker2
```

License
-------

MIT

Author Information
------------------

For more information, visit [talhajuikar.cloud](https://talhajuikar.cloud).
