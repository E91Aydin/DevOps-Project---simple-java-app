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

# Step 2.Installing and Configuring Apache Maven
## Introduction
This guide outlines the steps to install and configure Apache Maven on your Linux instance (Jenkins-server). Maven is an essential tool for building and managing Java projects. Follow these instructions to set it up correctly.
### 1.Download Maven Binary Archive and connect to Linux instance
Visit the Apache Maven download page and copy the link to the binary tar.gz archive for Linux. In this example, we'll use version 3.9.4: apache-maven-3.9.4-bin.tar.gz.SSH into your Linux instance (Jenkins-server) as the root user.
### 2.Download and Extract Maven and Move Maven Folder
Download the Maven archive:
```bash
wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz
```
Extract the archive:
```bash
tar -xzvf apache-maven-3.9.4-bin.tar.gz
```
After extracting, a folder named apache-maven-3.9.4 (or similar) will be created. Rename it to simply maven and move it:
```bash
mv apache-maven-3.9.4 maven
```
### 3.Set Environment Variables and Verify Installation
To run Maven from anywhere on the server, configure the environment variables. Navigate to the root home directory and open the .bash_profile file using a text editor:
```bash
cd ~
vim .bash_profile
```
Add the following lines at the end of the file (make sure to replace the Java path with the correct.Save the file and execute.
```bash
M2_HOME=/opt/maven
M2=/opt/maven/bin
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.19.0.7-1.amzn2.0.1.x86_64
PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2

source .bash_profile
```
Confirm that Maven and Java paths have been added to the PATH and check the installed Maven version.
```bash
echo $PATH
mvn -version
```