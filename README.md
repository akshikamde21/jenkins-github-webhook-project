# Jenkins-GitHub Integration Project
Automate your project builds with seamless integration between Jenkins and GitHub! This project provides a step-by-step guide to set up a robust CI/CD pipeline on an EC2 instance, allowing you to focus on development while Jenkins takes care of the heavy lifting.

## Installation Guide

### Step 1: Create an EC2 Instance
```bash
# Create an EC2 instance and update packages
sudo apt update
```
### Step 2: Install Jenkins
```bash
# Install Java and Jenkins
sudo apt install fontconfig openjdk-17-jre -y
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
sudo systemctl start jenkins.service
sudo systemctl enable jenkins.service
```
### Step 3: Configure Jenkins
Go to Security Groups of your ec2 instance and add custom TCP port as 8080 in inbound rules.
Access Jenkins on your EC2 instance at http://<your-ec2-public-ip>:8080.
Follow the setup wizard to create an admin user and finish the initial configuration.

### Step 4: Set Up Docker
```bash
# Install Docker and Docker Compose
sudo apt install docker.io -y
sudo systemctl start docker.service
sudo systemctl enable docker.service
sudo usermod -aG docker ubuntu
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
sudo systemctl start docker
sudo apt-get install docker-compose -y
```
### Step 5: Configure Jenkins Pipeline
Create a Jenkins pipeline for your project (e.g., django-todo-app) by forking the GitHub repository and configuring the pipeline script.
Add inbound rules for port 8000 in your EC2 instance's security group to access the application.

### Step 5: Set Up GitHub Webhook
Install GitHub integration plugin in Jenkins.
In your GitHub repository settings, navigate to "Webhooks" and add a new webhook.
Payload URL: http://<ec2-instance-ip>:8080/github-webhook/
Content type: application/json
Choose to send all events.
Enable the webhook.
Resfresh immediately to get a "tick" mark.

### Usage
Make changes to your project code on GitHub.
Commit and push your changes.
Jenkins will automatically detect the changes and trigger the pipeline.
Monitor the build status and access your deployed application at http://<ec2-instance-ip>:8000.

### Conclusion
With Jenkins-GitHub integration, you can streamline your development workflow and ensure reliable and consistent builds for your projects. Say goodbye to manual builds and hello to automated efficiency!
