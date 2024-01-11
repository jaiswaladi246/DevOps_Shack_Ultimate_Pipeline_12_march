Installing Kubernetes using `kubeadm` involves several steps. Here's a basic guide to set up a simple Kubernetes cluster from scratch using `kubeadm` on a Linux machine. This guide assumes you have three nodes: one master and two worker nodes.

### Prerequisites:

1. **Operating System:** Ubuntu 18.04 or later on all nodes.

2. **Hardware:** Ensure each node has at least 2GB RAM.

3. **Network:** Nodes should be able to communicate with each other.

### Step 1: Update System and Install Docker

```bash
# On all nodes (master and workers)
sudo apt update
sudo apt upgrade -y

# Install Docker
sudo apt install docker.io -y

# Enable and start Docker service
sudo systemctl enable docker
sudo systemctl start docker
```

### Step 2: Install kubeadm, kubelet, and kubectl

```bash
# On all nodes (master and workers)
sudo apt install apt-transport-https curl -y

# Add Kubernetes signing key
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

# Add the Kubernetes repository
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

# Update and install kubeadm, kubelet, and kubectl
sudo apt update
sudo apt install kubeadm kubelet kubectl -y
```

### Step 3: Disable Swap and Set Hostname (On all nodes)

```bash
# Disable Swap
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

# Set Hostname
sudo hostnamectl set-hostname <node-name>
```

Replace `<node-name>` with appropriate names like `k8s-master`, `k8s-worker-1`, and so on.

### Step 4: Initialize the Master Node

On the master node, run the following commands:

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

After the command completes, it will output a `kubeadm join` command. Save this command as you will use it later to join worker nodes.

### Step 5: Set Up kubeconfig for the Current User

Run the following commands on the master node:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Step 6: Install a Pod Network (On the Master Node)

Choose a Pod network provider. For example, you can use Calico:

```bash
kubectl apply -f https://docs.projectcalico.org/v3.20/manifests/calico.yaml
```

### Step 7: Join Worker Nodes

Run the `kubeadm join` command obtained from the master node on each worker node.

### Step 8: Verify the Cluster (On the Master Node)

```bash
kubectl get nodes
kubectl get pods --all-namespaces
```

This should show that the master and worker nodes are ready, and the essential pods are running.

Congratulations! You've set up a basic Kubernetes cluster using `kubeadm`. Keep in mind that this is a minimal configuration, and you might want to customize it further based on your specific requirements.
