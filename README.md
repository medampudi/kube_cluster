Here is the updated README file with the commands and attribution, excluding the full code files.

---

# Kubernetes Cluster Setup on Ubuntu 22.04 with Kubernetes 1.29 on Contabo VPS

This guide provides instructions to set up a Kubernetes cluster using `kubeadm` on Ubuntu 22.04 servers from Contabo VPS. The steps are based on a DigitalOcean tutorial but have been adapted for the updated environment and software versions. Additional inspiration and guidance were provided by the [kubernetes-playbooks](https://github.com/torgeirl/kubernetes-playbooks/tree/main) repository by Torgeir L. Hatlevik.

## Prerequisites

- Three Contabo VPS servers running Ubuntu 22.04.
- SSH access to each server with `root` privileges.
- Basic knowledge of Ansible for automating the setup process.

## Inventory Setup

Prepare your `hosts` file to list the master and worker nodes:

```ini
[masters]
master ansible_host=<MASTER_IP> ansible_user=root

[workers]
worker1 ansible_host=<WORKER1_IP> ansible_user=root
worker2 ansible_host=<WORKER2_IP> ansible_user=root
```

## Ansible Playbooks

### 1. Initial Setup

Create a user `ubuntu` and configure passwordless sudo access. The playbook is defined in `initial.yml`.

**Run the playbook:**
```sh
ansible-playbook -i hosts initial.yml
```

### 2. Install Kubernetes Dependencies

Install necessary dependencies and disable swap. The playbook is defined in `kube-dependencies.yml`.

**Run the playbook:**
```sh
ansible-playbook -i hosts kube-dependencies.yml
```

### 3. Master Node Initialization

Initialize the master node. The playbook is defined in `master.yml`.

**Run the playbook:**
```sh
ansible-playbook -i hosts master.yml
```

### 4. Worker Node Setup

Join worker nodes to the cluster. The playbook is defined in `worker.yml`.

**Run the playbook:**
```sh
ansible-playbook -i hosts worker.yml
```

## Verifying the Cluster

To verify the cluster setup, SSH into the master node and run:

```sh
kubectl get nodes
```

You should see a list of your master and worker nodes in the `Ready` state.

## Acknowledgments

This setup guide was inspired by:
- The [DigitalOcean tutorial](https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-ubuntu-20-04)
- The [kubernetes-playbooks](https://github.com/torgeirl/kubernetes-playbooks/tree/main) repository by Torgeir L. Hatlevik, which provided invaluable guidance, especially regarding Kubernetes dependencies and APT key setup.

---

This README provides a comprehensive guide to set up a Kubernetes cluster on Ubuntu 22.04 with Kubernetes 1.29 on Contabo VPS servers, leveraging Ansible for automation and ease of deployment.