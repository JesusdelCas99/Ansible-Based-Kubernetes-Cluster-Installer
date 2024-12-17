# Simplifying Kubernetes Cluster Deployment: An Ansible Automation Approach

This repository provides an Ansible-based solution to set up a Kubernetes cluster. The playbooks and roles included automate the installation and configuration of both control plane and worker nodes, including the setup of networking (Flannel CNI). 

### Prerequisites

- Ansible installed on the control machine.
- SSH access to all nodes (control plane and workers).
- Ubuntu-based nodes for the Kubernetes cluster.
- Python version consistency across all nodes.


### Playbooks

Playbooks consist of one or more roles and may include configuration parameters. Available playbooks include:
- `create-cluster`: Sets up the cluster, installs Kubernetes components, and joins worker nodes.
- `delete-cluster`: Cleans up the Kubernetes installation and frees up resources.

### Roles

The roles defined are:
- `setup-hosts`: Prepares the hosts (hostname, hosts file, APT updates).
- `setup-k8s`: Installs dependencies and configures the system for Kubernetes.
- `install-k8s`: Installs Kubernetes packages (kubeadm, kubelet, kubectl).
- `start-k8s`: Initializes the control plane on the master node and applies the Flannel network.
- `join-nodes`: Joins the worker nodes to the Kubernetes cluster.
- `purge-cluster`: Removes all Kubernetes resources and prunes container images.
- `purge-k8s`: Stops and disables Kubernetes services, removes packages, and cleans up config files.

### Execution

1. Run the `create-cluster.yaml` playbook to set up the Kubernetes cluster:

   ```
   ansible-playbook -i inventories/inventory.yaml playbooks/create-cluster.yaml --ask-become-pass
   ```

2. To remove the cluster, execute the `delete-cluster.yaml` playbook:

   ```
   ansible-playbook -i inventories/inventory.yaml playbooks/delete-cluster.yaml --ask-become-pass
   ```

### Configuration

Before running the playbooks, make sure to update the `inventory.yaml` file to define the control plane and worker nodes, along with the corresponding Kubernetes cluster configuration for deployment. 

Key configuration parameters include:

   ```
   kubernetes:
      k8s_version: "1.30"                        # Kubernetes version (includes kubelet, kubeadm, and kubectl versions)
      pod_network_cidr: "210.20.0.0/16"          # CIDR block for the pod network  
    flannel:
      version: "0.25.7"                          # Flannel CNI plugin version for Kubernetes networking
   ```

### Acknowledgments

This project is licensed under the MIT License. For additional guidance, see the official Kubernetes setup documentation at [Install and Set Up kubeadm](https://v1-30.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/).
