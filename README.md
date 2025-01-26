Ansible Role: Talhajuikar.kubernetes
=========
[![Push to Ansible Galaxy](https://github.com/TalhaJuikar/ansible-role-kubernetes/actions/workflows/publish.yml/badge.svg)](https://github.com/TalhaJuikar/ansible-role-kubernetes/actions/workflows/publish.yml)

Ansible role to set up a HA Kubernetes cluster with multiple masters and workers. Can also be used to set up a single master and multiple workers. Leverages kube-vip for virtual IP address and metallb for load balancing.
To set up a single master and multiple workers, just add the master node IP to the k8s_master group and worker nodes to the k8s_worker group in the inventory file. Leave the k8s_other_masters group and VIP variable empty. You can also choose not to deploy metallb by leaving the metallb_ip_range variable empty.

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
metallb_ip_range: "" # Range of IP addresses to be used by metallb for load balancing.
VIP: "" # Virtual IP address for the cluster to be used by kube-vip.
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

## How to use this role
1. Install ansible on your local machine.
2. Create an inventory file with the IP addresses of the master and worker nodes. (Recommended to set up key-based SSH authentication)
3. Install the role using ansible-galaxy
    ```bash
    ansible-galaxy install TalhaJuikar.kubernetes
    ```
4. Create a playbook with the example playbook below.
5. Run the playbook using the following command:
    ```bash
    ansible-playbook -i inventory playbook.yml
    ```
6. Upon successfull completion, you can find the kubeconfig file at /tmp/kubeconfig on your local machine unless you have set copy_kubeconfig to false.

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
    VIP: "192.168.200.50"
    metallb_ip_range: "192.168.203.50-192.168.203.60"
    kubernetes_pod_network:
      cni: 'flannel'
      cidr: '10.244.0.0/16'
```

Example Inventory
----------------

```ini
[k8s_master]
control-1 ansible_host=IP_ADDRESS
[k8s_other_masters]
control-2 ansible_host=IP_ADDRESS
control-3 ansible_host=IP_ADDRESS

[k8s_worker]
worker1 ansible_host=IP_ADDRESS
worker2 ansible_host=IP_ADDRESS
```

License
-------

MIT

Author Information
------------------

For more information, visit [talhajuikar.cloud](https://talhajuikar.cloud).
