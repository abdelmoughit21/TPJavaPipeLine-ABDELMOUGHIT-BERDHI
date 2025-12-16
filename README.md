# TPJavaPipeLine-ABDELMOUGHIT-BERDHI

## 📋 Project Description

This project demonstrates the use of Jenkins Pipeline with Docker to automate the build and testing of a Java Maven application.

## 🛠️ Prerequisites

Before starting, make sure you have installed:

- **Docker** (version 20.10 or higher)
- **Git**
- **Jenkins** (via Docker)

## 📦 Installation

### 1. Install Docker

#### On Windows:
- Download [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop)
- Install and restart your computer

### 2. Build the Maven-Git Docker Image

```bash
# Navigate to the project folder
cd TPJavaPipeLine-ABDELMOUGHIT-BERDHI

# Build the Docker image
docker build -t my-maven-git:latest -f Dockerfile.maven .
```

### 3. Install Jenkins with Docker

```bash
# Run Jenkins with Docker access (port 9090 if 8080 is in use)
docker run -d \
  --name jenkins \
  -p 9090:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts
```

**Note:** We use port 9090 instead of 8080 to avoid conflicts with other applications.

Get the initial admin password:
```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

### 4. Install Docker CLI in Jenkins Container

Jenkins needs Docker CLI to run Docker containers:

```bash
# Access Jenkins container as root
docker exec -u root -it jenkins bash

# Install Docker CLI
apt-get update
apt-get install -y docker.io

# Add jenkins user to docker group
usermod -aG docker jenkins

# Fix socket permissions
chmod 666 /var/run/docker.sock

# Exit container
exit

# Restart Jenkins
docker restart jenkins
```

Verify Docker works:
```bash
docker exec -it jenkins docker ps
```

### 5. Configure Jenkins

1. Open your browser: `http://localhost:9090`
2. Enter the initial admin password from step 3
3. Install suggested plugins 
4. Create an admin user
5. Click "Start using Jenkins"

### 6. Add GitHub Credentials (Required)

The pipeline needs credentials to clone the repository:

1. Go to **Manage Jenkins** > **Manage Credentials**
2. Click **(global)** domain
3. Click **Add Credentials**
4. Fill in:
   - **Kind**: Username with password
   - **Username**: Your GitHub username
   - **Password**: Your GitHub Personal Access Token
   - **ID**: `github-creds` (important!)
   - **Description**: GitHub Credentials
5. Click **Create**

**To get a Personal Access Token:**
- Go to GitHub Settings > Developer settings > Personal access tokens > Tokens (classic)
- Generate new token with "repo" scope
- Copy and use as password

## 🚀 Usage

### Create Pipeline in Jenkins

1. In Jenkins, click **"New Item"**
2. Name your pipeline: `JavaPipeline`
3. Select **"Pipeline"**
4. Click **OK**
5. Scroll to **Pipeline** section
6. Definition: **"Pipeline script"**
7. Copy and paste the entire `Jenkinsfile` content into the script box
8. Click **Save**

### Run the Pipeline

1. Click **"Build Now"**
2. Watch execution in **Build History**
3. Click on build number (e.g., #1)
4. Click **"Console Output"** to see detailed logs

The pipeline will execute 3 stages:
- **Checkout**: Clone the Java Maven repository
- **Build**: Compile and test with Maven
- **Run**: Execute the compiled JAR file

## 📁 Project Structure

```
TPJavaPipeLine-ABDELMOUGHIT-BERDHI/
├── README.md                 # This file
├── Jenkinsfile              # Jenkins pipeline script
├── Dockerfile.maven         # Docker image with Maven + Git
├── .gitignore              # Git ignore file
```

## 🔄 Pipeline Stages

### Stage 1: Checkout
- Cleans the workspace
- Clones the Java Maven repository from GitHub
- Uses credentials stored in Jenkins

### Stage 2: Build
- Navigates to the project directory
- Runs `mvn clean test package` to compile and test
- Creates a JAR file in the target/ directory

### Stage 3: Run
- Executes the compiled JAR file
- Displays application output
---

## Quick Start Commands

```bash
# Build Maven-Git image
docker build -t my-maven-git:latest -f Dockerfile.maven .

# Run Jenkins
docker run -d --name jenkins -p 9090:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:lts

# Get Jenkins password
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

# Install Docker in Jenkins
docker exec -u root -it jenkins bash
apt-get update && apt-get install -y docker.io
usermod -aG docker jenkins
chmod 666 /var/run/docker.sock
exit

# Restart Jenkins
docker restart jenkins

# Access Jenkins at: http://localhost:9090
