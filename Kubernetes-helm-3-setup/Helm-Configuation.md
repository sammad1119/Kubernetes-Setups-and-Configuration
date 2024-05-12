## Kubernetes cluster  helm 3 Configuration and setup steps.

## Overview
This guide provides detailed instructions for setting up a Kubernetes cluster using Kubeadm. The guide includes instructions for installing and configuring containerd and Kubernetes, disabling swap, initializing the cluster, installing Flannel, and joining nodes to the cluster.

## Installing Helm
This guide shows how to install the Helm CLI. Helm can be installed either from source, or from pre-built binary releases.

## From the Binary Releases
Every release of Helm provides binary releases for a variety of OSes. These binary versions can be manually downloaded and installed.

- Download your desired version
- Unpack it (tar -zxvf helm-v3.0.0-linux-amd64.tar.gz)
- Find the helm binary in the unpacked directory, and move it to its desired destination (mv linux-amd64/helm /usr/local/bin/helm)
  
From there, you should be able to run the client and add the stable repo: helm help.

## Note:

Helm automated tests are performed for Linux AMD64 only during CircleCi builds and releases. Testing of other OSes are the responsibility of the community requesting Helm for the OS in question.

## From Script
Helm now has an installer script that will automatically grab the latest version of Helm and install it locally.

You can fetch that script, and then execute it locally. It's well documented so that you can read through it and understand what it is doing before you run it.

```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

## Install containerd
To install Containerd and Docker, use the following commands:

```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install containerd.io -y
```
# Check Docker version 

```
docker --version
```

# Check Docker version if not then install using the Command

```
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


## Create containerd configuration
Next, create the containerd configuration file using the following commands:

```
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

## Edit /etc/containerd/config.toml
Edit the containerd configuration file to set SystemdCgroup to true. Use the following command to open the file:

```
sudo nano /etc/containerd/config.toml
```

Set SystemdCgroup to true:
```
SystemdCgroup = true
```

Restart containerd:
```
sudo systemctl restart containerd
```

## Install Kubernetes
To install Kubernetes, use the following commands:

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update 
sudo apt install kubeadm kubelet kubectl kubernetes-cni
```

## Disable swap
Disable swap using the following command:

```
sudo swapoff -a
```

If there are any swap entries in the /etc/fstab file, remove them using a text editor such as nano:
```
sudo nano /etc/fstab
```

Enable kernel modules
```
sudo modprobe br_netfilter
```

Add some settings to sysctl
```
sudo sysctl -w net.ipv4.ip_forward=1
```
## Initialize the Cluster (Run only on master)
Use the following command to initialize the cluster:
```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

Create a .kube directory in your home directory:
```
mkdir -p $HOME/.kube
```

Copy the Kubernetes configuration file to your home directory:
```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```

Change ownership of the file:
```
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Install Flannel (Run only on master)
Use the following command to install Flannel:
```
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/v0.20.2/Documentation/kube-flannel.yml
```

## Verify Installation
Verify that all the pods are up and running:

```
kubectl get pods --all-namespaces
```

## Join Nodes (Example Token give token that master node has given)
To add nodes to the cluster, run the kubeadm join command with the appropriate arguments on each node. The command will output a token that can be used to join the node to the cluster.
```
sudo kubeadm join 192.168.4.50:6443 --token gskopd.a4gg38hvsql2qprz         --discovery-token-ca-cert-hash sha256:535a4ec84c541a6401f167fa5f2ff4717a359230fe3af6a81cd8ed1d427ce169
```
## Test the setup
```
kubectl get nodes
NAME       STATUS   ROLES           AGE     VERSION
master     Ready    control-plane   3m59s   v1.28.9
slave-01   Ready    <none>          3m19s   v1.28.9

******
```
```
kubectl get pods -A
```

## Run a demo app
```
kubectl run nginx --image=nginx --port=80 
kubectl expose pod nginx --port=80 --type=NodePort
```

## References
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
- https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/
- https://www.mirantis.com/blog/how-install-kubernetes-kubeadm/
- https://www.mankier.com/1/kubeadm-init
- https://kubernetes.io/docs/setup/production-environment/container-runtimes/#docker
- https://github.com/containerd/containerd/blob/main/docs/getting-started.md
- https://kubernetes.io/docs/reference/networking/ports-and-protocols/
- https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#install
- https://github.com/skooner-k8s/skooner
- https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#eks
- https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md
- https://www.mankier.com/1/kubeadm-init
