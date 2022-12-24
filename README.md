# Build a Kubernetes cluster using k3s via Ansible

Author: <https://github.com/itwars>

## K3s Ansible Playbook

Build a Kubernetes cluster using Ansible with k3s. The goal is easily install a Kubernetes cluster on machines running:

- [X] Debian
- [X] Ubuntu
- [X] CentOS
- [X] Alma Linux https://almalinux.org/

on processor architecture:

- [X] x64
- [X] arm64
- [X] armhf

## System requirements

Deployment environment must have Ansible 2.4.0+
Master and nodes must have passwordless SSH access

## Usage

First create a new directory based on the `sample` directory within the `inventory` directory:

```bash
cp -R inventory/sample inventory/my-cluster
```

Second, edit `inventory/my-cluster/hosts.ini` to match the system information gathered above. For example:

```bash
[master]
192.16.35.12

[node]
192.16.35.[10:11]

[k3s_cluster:children]
master
node
```

If needed, you can also edit `inventory/my-cluster/group_vars/all.yml` to match your environment.

Start provisioning of the cluster using the following command:

```bash
ansible-playbook site.yml -i inventory/g01/hosts.ini

```

## patching
```bash
ansible-playbook patch.yml  -i inventory/g01/hosts.ini 
```
## Kubeconfig

To get access to your **Kubernetes** cluster just

```bash
scp master_ip:~/.kube/config k3s_kubeconfig

export KUBECONFIG=k3s_kubeconfig

oc get nodes -o wide                                                                                                                                                     master 
NAME            STATUS   ROLES                  AGE   VERSION        INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION              CONTAINER-RUNTIME
alma-master01   Ready    control-plane,master   20h   v1.25.4+k3s1   10.0.0.191    <none>        AlmaLinux 8.7 (Stone Smilodon)   4.18.0-425.3.1.el8.x86_64   containerd://1.6.8-k3s1
alma-node01     Ready    <none>                 20h   v1.25.4+k3s1   10.0.0.190    <none>        AlmaLinux 8.7 (Stone Smilodon)   4.18.0-425.3.1.el8.x86_64   containerd://1.6.8-k3s1
alma-node02     Ready    <none>                 20h   v1.25.4+k3s1   10.0.0.192    <none>        AlmaLinux 8.7 (Stone Smilodon)   4.18.0-425.3.1.el8.x86_64   containerd://1.6.8-k3s1

```

## misc
```bash
ANSIBLE_STDOUT_CALLBACK=yaml  ansible -i inventory/g01/hosts.ini all  -m shell -a "free -g"
```