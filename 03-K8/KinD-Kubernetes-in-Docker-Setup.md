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

