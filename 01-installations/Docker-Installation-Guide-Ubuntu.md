# Docker Installation Guide for Ubuntu 24.04.4 LTS

This guide provides step-by-step instructions to install Docker Engine and Docker Compose on a fresh Ubuntu 24.04.4 LTS system.

**Important:** All commands require `sudo` unless you are logged in as root. This guide assumes you are running these commands from a terminal with `sudo` privileges.

---

## Prerequisites
- Ubuntu 24.04.4 LTS (or any 24.04 variant)
- Internet connection
- Sudo/root access to the machine
- A GitHub account (for PAT token, if pushing later)

---

## Step 1: Uninstall Old or Conflicting Versions
Ubuntu may come with older, conflicting Docker packages in its default repositories. Remove them to ensure a clean installation.

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt remove $pkg; done

Step 2: Set Up Docker's Official APT Repository
sudo apt update
sudo apt install ca-certificates curl

#2.2 Add Docker's Official GPG Key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

#2.3 Add the Docker Repository to APT Sources
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

#2.4 Update the APT Package Index
sudo apt update

#tep 3: Install Docker Engine

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

#4.1 Check Service Status
sudo systemctl status docker

#4.2 Run the Hello-World Test Container
sudo docker run hello-world

#Step 5: Post-Installation (Run Docker Without sudo)

Add User to Docker Group
sudo groupadd docker          # (The group usually exists, but this ensures it does)
sudo usermod -aG docker $USER

#5.2 Apply Group Changes
newgrp docker

#5.3 Fix Permission Issues (if any)

sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R

#Verify Non-Sudo Access
docker run hello-world


#Step 6: Enable Docker to Start on System Boot

sudo systemctl enable docker.service
sudo systemctl enable containerd.service

#Verify Docker Compose Installation
docker compose version






Important: The command is docker compose (with a space). The older standalone binary docker-compose (with a hyphen) is not installed by this method, but the plugin is the official, recommended modern equivalent.

Troubleshooting Common Issues
Issue	Solution
Permission denied when running docker	Ensure you completed Step 5 and restarted your session (newgrp docker or logout/login).
docker: command not found	Verify that /usr/bin/docker exists. If not, re-run Step 3 installation commands.
Repository GPG key errors	Re-download the GPG key in Step 2.2 and run sudo apt update again.
Error cannot connect to the Docker daemon	Run sudo systemctl start docker to start the service, then enable it with Step 6.



















