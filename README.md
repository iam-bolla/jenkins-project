# jenkins-project
# Jenkins with Docker Agents on EC2

Tired of manually patching Jenkins worker nodes, installing dependencies, or managing version conflicts? You're in the right place. This guide walks you through configuring Docker containers as Jenkins worker nodes (agents) — clean, disposable, and consistent environments for every build.

## 🚀 What’s Covered

- Installing Jenkins on an EC2 Instance
- Configuring Docker on the same EC2
- Granting Jenkins permission to use Docker
- Installing Docker Pipeline Plugin
- Running Jenkins builds inside Docker containers using `pipeline` scripts

---

## 🖥️ 1. Launch EC2 & Install Jenkins

### 📦 Step 1: Install Java (Jenkins prerequisite)
```bash
sudo apt update
sudo apt install openjdk-17-jre -y
```
 Step 2: Install Jenkins
```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```
 Step 3: Configure Security Groups
Open port 8080 in EC2 inbound rules.

 Step 4: Unlock Jenkins
Visit http://<your-ec2-ip>:8080 in a browser.

Retrieve the Jenkins admin password:
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Install suggested plugins and optionally create an admin user.
 2. Install Docker on EC2
```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```
 Grant Permissions to Jenkins & Ubuntu Users
```bash
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
sudo systemctl restart docker
```
Note: If you're setting up a remote slave EC2, repeat these steps on that instance.
 3. Install Docker Pipeline Plugin
Inside Jenkins:

Go to Manage Jenkins > Manage Plugins

Search for Docker Pipeline

Install and restart Jenkins

This plugin allows Jenkins to:

Pull images from Docker Hub

Mount workspace inside containers

Run pipeline steps inside containers

Clean up after execution
Back in Jenkins:

Click New Item > Pipeline

Set "Pipeline script from SCM"

Choose Git and add your repo URL

Set branch as main and script path as Jenkinsfile

Save and Build Now
You should see the Node.js version output — meaning Docker agent setup is successful.
