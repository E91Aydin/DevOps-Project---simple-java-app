# DevOps Project - CI CD Pipeline | Deploy to Kubernetes Cluster Using Jenkins
![](images/Draw%20Ready.JPG)
# Pre-requisites
* Launched instances on AWS EC2
* AWS account and keypair created in AWS
* Preferred IDE (use VS Code)
* AWS region us-east-1
* Test java app files added into Git repo

# Step 1.Setting Up and Configuring Jenkins on AWS Linux
## Introduction
This guide will walk you through the process of setting up and configuring Jenkins on an AWS Linux instance named "Jenkins-server." Make sure you have a key pair ready to connect to the instance via SSH. Additionally, you'll need to adjust the AWS Security Group settings to allow incoming connections on port 8080, as Jenkins operates on this port.
## Installation Steps
### 1.Launch AWS Instance
Begin by launching an AWS Linux instance named "Jenkins-server." Follow the installation tutorial provided. Remember to have your key pair ready for SSH access.
### 2.Configure Security Group
Navigate to the AWS Security Group settings for the Jenkins-server instance and allow inbound traffic on port 8080. This is necessary for Jenkins to function properly.
### 3.Accessing Jenkins
After successfully creating the Jenkins instance, locate the public IP address of the server. Open a new browser tab and enter the IP address followed by port 8080 (e.g., http://54.166.200.233:8080). This will take you to the Jenkins page.
### 4.Setting Initial Admin Password
On the Jenkins page, you will be prompted to provide an initial admin password. SSH into your Jenkins-server instance and retrieve the password using the following command:
 
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
Copy the password and paste it into the prompt on the Jenkins page.
### 5.Customizing Jenkins and User Registration
Once the password is entered correctly, you will have the opportunity to customize your Jenkins setup according to your preferences.Following customization, you'll need to register as a user. This step is essential for utilizing Jenkins for your DevOps projects.