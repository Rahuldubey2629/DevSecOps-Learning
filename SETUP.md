# DevSecOps Tools Setup Guide

This guide covers the installation and configuration of Jenkins and SonarQube for the DevSecOps pipeline.

## Table of Contents
- [Jenkins Installation](#jenkins-installation)
- [SonarQube Setup](#sonarqube-setup)
- [Jenkins Plugins Installation](#jenkins-plugins-installation)
- [Tool Configuration in Jenkins](#tool-configuration-in-jenkins)

---

## Jenkins Installation

### Prerequisites
- Ubuntu/Debian-based Linux system
- Java 21 (OpenJDK)

### Step 1: Update System and Install Java

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre
```

Verify Java installation:
```bash
java -version
```

Expected output:
```
openjdk version "21.x.x" 2024-xx-xx
OpenJDK Runtime Environment (build 21.x.x)
OpenJDK 64-Bit Server VM (build 21.x.x, mixed mode, sharing)
```

### Step 2: Add Jenkins Repository

Download Jenkins GPG key:
```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```

Add Jenkins repository to sources list:
```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### Step 3: Install Jenkins

```bash
sudo apt update
sudo apt install jenkins
```

### Step 4: Start Jenkins Service

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```

### Step 5: Access Jenkins

1. Open your browser and navigate to: `http://localhost:8080` or `http://<your-server-ip>:8080`

2. Get the initial admin password:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

3. Copy the password and paste it in the Jenkins setup wizard

4. Install suggested plugins

5. Create your first admin user

6. Configure Jenkins URL (use default or customize)

### Additional Resources
For more detailed installation instructions, visit: [Jenkins Official Documentation](https://www.jenkins.io/doc/book/installing/linux/)

---

## SonarQube Setup

### Prerequisites
- Docker installed on your system

### Step 1: Pull and Run SonarQube Container

Run SonarQube using Docker:
```bash
docker run -d --name sonarqube -p 9000:9000 sonarqube:community
```

**Note:** Check Docker Hub for the latest community edition: `sonarqube:community` or `sonarqube:lts-community`

### Step 2: Access SonarQube

1. Wait for the container to start (30-60 seconds)

2. Open your browser and navigate to: `http://localhost:9000`

3. Default credentials:
   - Username: `admin`
   - Password: `admin`

4. You will be prompted to change the password on first login

### Step 3: Verify SonarQube is Running

Check running containers:
```bash
docker ps
```

You should see the SonarQube container running:

![SonarQube Running](docs/images/image-1.png)

Check all running services (Jenkins + SonarQube):

![Jenkins and Docker Services](docs/images/image-2.png)

### Step 4: Generate SonarQube Token

1. Login to SonarQube
2. Go to: **My Account** → **Security** → **Generate Tokens**
3. Enter a token name (e.g., `jenkins-sonar-token`)
4. Click **Generate**
5. **Save this token** - you'll need it for Jenkins integration

### Step 5: Configure SonarQube Project

1. Click **Create Project** → **Manually**
2. Enter Project Key: `NodeJS-Project`
3. Enter Display Name: `NodeJS-Project`
4. Click **Set Up**
5. Choose **Locally**
6. Generate a token or use existing one
7. Select **Other** for build technology
8. Note the project key for Jenkins configuration

---

## Jenkins Plugins Installation

After Jenkins is installed, install the following required plugins:

### Navigate to Plugin Manager
1. Go to: **Manage Jenkins** → **Manage Plugins** → **Available**

### Required Plugins

#### Core Plugins
- **NodeJS Plugin** - For Node.js project builds
- **Docker Plugin** - For Docker operations
- **Docker Pipeline Plugin** - For Docker pipeline integration

#### Security & Quality Plugins
- **SonarQube Scanner Plugin** - For code quality analysis
- **GitLeaks Plugin** (if available) - For secret scanning
- **Trivy Plugin** (if available) - For security scanning

#### Source Control
- **Git Plugin** - For Git integration (usually pre-installed)
- **GitHub Plugin** - For GitHub integration

#### General
- **Pipeline Plugin** - For declarative pipelines (usually pre-installed)
- **Credentials Plugin** - For managing credentials (usually pre-installed)
- **Workspace Cleanup Plugin** - For cleaning workspace

### Installation Steps
1. Search for each plugin
2. Check the checkbox next to the plugin name
3. Click **Install without restart** or **Download now and install after restart**
4. Restart Jenkins if required

---

## Tool Configuration in Jenkins

### 1. Configure NodeJS

1. Go to: **Manage Jenkins** → **Global Tool Configuration**
2. Scroll to **NodeJS** section
3. Click **Add NodeJS**
4. Configuration:
   - Name: `nodejs23` (must match the name in Jenkinsfile)
   - Version: Select Node.js 23.x or your preferred version
   - Install automatically: ✓ Checked
5. Click **Save**

### 2. Configure SonarQube Scanner

1. Go to: **Manage Jenkins** → **Global Tool Configuration**
2. Scroll to **SonarQube Scanner** section
3. Click **Add SonarQube Scanner**
4. Configuration:
   - Name: `sonarqube-scanner` (must match SCANNER_HOME in Jenkinsfile)
   - Install automatically: ✓ Checked
   - Version: Select latest version
5. Click **Save**

### 3. Configure SonarQube Server

1. Go to: **Manage Jenkins** → **Configure System**
2. Scroll to **SonarQube servers** section
3. Check **Environment variables** → **Enable injection of SonarQube server configuration**
4. Click **Add SonarQube**
5. Configuration:
   - Name: `sonar-server` (must match withSonarQubeEnv in Jenkinsfile)
   - Server URL: `http://localhost:9000` or `http://<sonarqube-ip>:9000`
   - Server authentication token: Add the token generated from SonarQube
     - Click **Add** → **Jenkins**
     - Kind: **Secret text**
     - Secret: Paste your SonarQube token
     - ID: `sonar-token`
     - Description: `SonarQube Token`
6. Click **Save**

### 4. Configure Docker Credentials

1. Go to: **Manage Jenkins** → **Manage Credentials**
2. Click on **(global)** domain
3. Click **Add Credentials**
4. Configuration:
   - Kind: **Username with password**
   - Username: Your Docker Hub username
   - Password: Your Docker Hub password or access token
   - ID: `docker-cred` (must match credentialsId in Jenkinsfile)
   - Description: `Docker Hub Credentials`
5. Click **Create**

### 5. Install Additional Tools on Jenkins Server

These tools need to be installed on the Jenkins server itself:

#### Install GitLeaks
```bash
# Download and install gitleaks
wget https://github.com/gitleaks/gitleaks/releases/download/v8.18.1/gitleaks_8.18.1_linux_x64.tar.gz
tar -xzf gitleaks_8.18.1_linux_x64.tar.gz
sudo mv gitleaks /usr/local/bin/
sudo chmod +x /usr/local/bin/gitleaks
gitleaks version
```

#### Install Trivy
```bash
# Add Trivy repository
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo "deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list

# Install Trivy
sudo apt-get update
sudo apt-get install trivy

# Verify installation
trivy --version
```

#### Install Docker
```bash
# Install Docker if not already installed
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Add Jenkins user to docker group
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

#### Install Docker Compose
```bash
# Install docker-compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

---

## Verification

After completing the setup, verify all tools are configured correctly:

### Verify Jenkins Tools
```bash
# From Jenkins server
node --version
npm --version
docker --version
docker-compose --version
gitleaks version
trivy --version
```

### Verify SonarQube
- Access SonarQube at `http://localhost:9000`
- Login with admin credentials
- Check that the project `NodeJS-Project` exists or can be created

### Verify Jenkins Configuration
1. Go to **Manage Jenkins** → **Global Tool Configuration**
2. Verify NodeJS and SonarQube Scanner are configured
3. Go to **Manage Jenkins** → **Configure System**
4. Verify SonarQube server is configured
5. Go to **Manage Jenkins** → **Manage Credentials**
6. Verify Docker credentials and SonarQube token are added

---

## Troubleshooting

### Jenkins Won't Start
```bash
# Check Jenkins status
sudo systemctl status jenkins

# Check Jenkins logs
sudo journalctl -u jenkins -f

# Restart Jenkins
sudo systemctl restart jenkins
```

### SonarQube Container Issues
```bash
# Check container logs
docker logs sonarqube

# Restart container
docker restart sonarqube

# Remove and recreate container
docker rm -f sonarqube
docker run -d --name sonarqube -p 9000:9000 sonarqube:community
```

### Permission Issues with Docker
```bash
# Add jenkins user to docker group
sudo usermod -aG docker jenkins

# Restart Jenkins
sudo systemctl restart jenkins
```

### Port Already in Use
```bash
# Check what's using port 8080 (Jenkins)
sudo netstat -tulpn | grep 8080

# Check what's using port 9000 (SonarQube)
sudo netstat -tulpn | grep 9000
```

---

## Next Steps

Once you've completed this setup, proceed to [PIPELINE_GUIDE.md](./PIPELINE_GUIDE.md) to learn how to configure and run the DevSecOps pipeline.
