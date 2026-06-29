# KinD (Kubernetes in Docker) Installation Guide for Ubuntu 24.04.4 LTS
## Prerequisites
- Ubuntu 24.04.4 LTS (or any 24.04 variant)
- Internet connection
- Sudo/root access to the machine
- **Docker** installed and running[reference:1]
- Docker service enabled and accessible (with or without `sudo`)


## Step 1: Install `kubectl` (Kubernetes Command-Line Tool)
`kubectl` is the Kubernetes command-line tool that allows you to interact with your cluster[reference:2].

### 1.1 Download the Latest Stable Version of `kubectl`
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

#Make the Binary Executable

bash
chmod +x ./kubectl

#1.3 Move kubectl to a Directory in Your PATH
sudo mv ./kubectl /usr/local/bin/kubectl

#1.4 Verify kubectl Installation
kubectl version --client


Step 2: Install KinD (Kubernetes in Docker)
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.24.0/kind-linux-amd64

chmod +x ./kind

#2.3 Move kind to a Directory in Your PATH
sudo mv ./kind /usr/local/bin/kind


kind --version

#3.1 Create a Single-Node Cluster (Default)
For a basic, single-node cluster:

kind create cluster



#3.2 Verify the Cluster is Running

kind get clusters


#3.3 Check Cluster Information with kubectl
kubectl cluster-info

#3.4 Check Node Status3
kubectl get nodes

#Check Kubernetes Version
kubectl version


#Step 4: (Optional) Create a Multi-Node Cluster
KinD allows you to create multi-node clusters with multiple workers.

#4.1 Create a Cluster Configuration File
#Create a file named kind-config.yaml:

nano kind-config.yaml

Paste the following configuration for a 4-node cluster (1 control-plane, 3 workers):

yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker


#4.2 Start the Multi-Node Cluster
kind create cluster --config=kind-config.yaml

#4.3 Verify the Multi-Node Cluster
kubectl get nodes

#Expected output:

text
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   2m    v1.xx.x
kind-worker          Ready    <none>          1m    v1.xx.x
kind-worker2         Ready    <none>          1m    v1.xx.x
kind-worker3         Ready    <none>          1m    v1.xx.x

#4.4 List Docker Containers (Nodes)
#Since KinD nodes are Docker containers, you can list them with:

docker ps


#Expected output:

text
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                      NAMES
xxxxxxxxxxxx   kindest/node:v1.xx.x   "/usr/local/bin/entr…"   2 minutes ago   Up 2 minutes   127.0.0.1:xxxxx->6443/tcp   kind-control-plane
xxxxxxxxxxxx   kindest/node:v1.xx.x   "/usr/local/bin/entr…"   1 minute ago    Up 1 minute                               kind-worker
xxxxxxxxxxxx   kindest/node:v1.xx.x   "/usr/local/bin/entr…"   1 minute ago    Up 1 minute                               kind-worker2
xxxxxxxxxxxx   kindest/node:v1.xx.x   "/usr/local/bin/entr…"   1 minute ago    Up 1 minute                               kind-worker3

#Step 5: Managing Your KinD Cluster
5.1 List All KinD Clusters
kind get clusters


#5.2 Delete a Cluster
kind delete cluster

#To delete a specific cluster by name:

kind delete cluster -n <cluster-name>


#5.3 Set kubectl Context to Your KinD Cluster
#By default, KinD automatically sets the context. If you need to set it manually:

kubectl config use-context kind-kind










































