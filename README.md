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

## Install Java And Jenkins:
so first install java

```
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version

```
and  install jenkins   
```
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins 
```


to run jenkins so run command :
``` sudo systemctl enable jenkins```


## RUN SonarQube using Docker:

```
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -v sonarqube_data:/opt/sonarqube/data -v sonarqube_extensions:/opt/sonarqube/extensions -v sonarqube_logs:/opt/ssonarqube/logs sonarqube:latest
```

## Plugin installation in Jenkins for Sonarqube integration 

- sonarqube scanner
- Sonar quality gate
- Docker

## How to connect  Sonarqube with  Jenkins

✅ Prerequisites:

    - Jenkins already installed and running
    - SonarQube running
    - Internet access to install plugins

 ### 🔐 Step 1: Create SonarQube Token

```
Open SonarQube in browser → http://100.24.23.144:8080

Login as admin

Click on your profile icon → My Account

Go to Security tab

Under Generate Tokens:

Name: jenkins-token

Click Generate

Copy the token somewhere safe

✅ This token will be used in Jenkins to connect to SonarQube.

```

##🔧 Step 2: Add SonarQube Token to Jenkins

```
Go to: Manage Jenkins > Manage Credentials

Select: (global) > Add Credentials

Select:

Kind: Secret text

Secret: Paste your SonarQube token

ID: sonar-token

Description: SonarQube Token for Jenkins

✅ This saves your token securely.

```

## ⚙️ Step 3: Configure SonarQube in Jenkins:

```
Go to: Manage Jenkins > Configure System

Scroll to SonarQube servers

Click Add SonarQube

Fill:

Name: SonarQube

Server URL: jenkinsserevr IP:8080

Server Authentication Token:

Click Add → Kind: Secret text → Choose sonar-token

✅ Tick: "Enable injection of SonarQube server configuration..."

Click Save.

```

## 🔨 Step 4: Configure SonarQube Scanner Tool:

```
Go to: Manage Jenkins > Global Tool Configuration

Scroll to SonarQube Scanner

Click Add SonarQube Scanner

Fill:

Name: SonarScanner

✅ Check: Install automatically

Click Save

```

