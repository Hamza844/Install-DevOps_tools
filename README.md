# 🛠️ DevOps Tools Installation Guide
This repository contains step-by-step scripts and instructions to install various DevOps tools on a Linux system (mainly Ubuntu-based). Each section contains a Bash script and usage guidance.

## Installation of Docker and Docker compose 
nano install-docker.sh

```
#!/bin/bash

set -e

echo "🚀 Updating system..."
sudo apt-get update -y
sudo apt-get upgrade -y

echo "🐳 Installing Docker dependencies..."
sudo apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

echo "🔐 Adding Docker GPG key..."
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "📦 Setting up Docker repository..."
echo \
  "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

echo "🔄 Updating apt cache..."
sudo apt-get update -y

echo "📥 Installing Docker Engine and Docker Compose plugin..."
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

echo "✅ Enabling and starting Docker service..."
sudo systemctl enable docker
sudo systemctl start docker

echo "👤 Adding current user to docker group (you may need to log out and back in)..."
sudo usermod -aG docker $USER

echo "🎉 Docker and Docker Compose installation complete!"
docker --version
docker compose version

```

Make it executable then run

```
chmod +x filename
```

Run the script:

```
./install-docker.sh
```

### ***Trivy***

```
sudo apt install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo "deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt update && sudo apt install trivy -y

```
✅ Scan Example:

```trivy image nginx:latest```


## Plugin installation in Jenkins for Sonarqube integration 
- sonarqube scanner
- Sonar quality gate
- Docker


## Connect Jenkins with sonarQube 

so frirst Create the Webhook token on SonarQube 
