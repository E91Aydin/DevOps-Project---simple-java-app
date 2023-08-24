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
# Step 3.Installing Maven Plugin and Configuring Jenkins for Maven
## 1.Install Maven Plugin and Configure JDK
* Access Jenkins by navigating to http://<your-JENKINS-server-IP>:8080 and log in.
* Go to "Manage Jenkins" > "Manage Plugins" > "Available" tab.
* Search for the "Maven Integration" plugin and install it without restarting Jenkins.
* Next, navigate to "Manage Jenkins" > "Global Tool Configuration" > "JDK."
* Add a new JDK configuration:
    * Name: java11
    * JAVA_HOME: /usr/lib/jvm/java-11-openjdk-11.0.19.0.7-1.amzn2.0.1.x86_64
(Add the correct path from your JENKINS_SERVER)
    * Ignore any error messages and save the configuration.
* Similarly, add Maven to the tools:
    * Name: Maven
    * MAVEN_HOME: /opt/maven
    * Uncheck "Install automatically" since you've already installed Maven.
    * Save your changes.
## 2.Install and Configure GitHub Plugin and Git on Jenkins Server
* Go to "Manage Jenkins" > "Manage Plugins" > "Installed" tab.
* Search for "GitHub" and disable "GitHub Branch Source Plugin" while enabling "GitHub Plugin."
* Restart Jenkins to apply the changes.
On your JENKINS_SERVER, install Git using the command:
```bash
sudo yum install git
```
## 3.Create and Test a Maven Build Project
* Return to the Jenkins dashboard.
* Click "New Item" to create a new project:
    * Name: Test-Maven-Build
    * Choose "Maven Project" and click "OK."
* In the project settings:
     * -Add a description: "Test Maven Build."
     * Under "Source Code Management," choose "Git" and add your Git repository's URL.
    * In "Goals and options," enter: clean install.
    * Apply the settings and save the project.
## 4.Initiate and Monitor the Build
* Click "Build Now" to initiate the build process.
* Monitor the build progress and check the console output.
* Once the build is successful, navigate to "Dashboard" > "Test-Maven-Build" > "Workspace."
* Inside the workspace, explore the path: Webapp/target/ to find the webapp.war build artifact.

# Step 4.Ansible Server Setup and Ansible Installation and Integrate with Jenkins
This guide outlines the process of setting up an Ansible server and installing Ansible for streamlined infrastructure management. It also covers integrating Ansible with Jenkins to automate tasks and deployments.
## Server Setup and User Configuration
### Launch AWS Linux Instance

* Launch an AWS Linux instance named "ANSIBLE-SERVER."
* Change the hostname by editing /etc/hostname and replacing it with "ansible-server."
* Reboot the server using init 6.
### Create New User and Grant Sudo Access

* Create a new user: ansadmin.
* Set a password for the user: passwd ansadmin.
* Grant sudo privileges to the user by editing the sudoers file: visudo.
### Enable Password Authentication

* Modify SSH configuration: vi /etc/ssh/sshd_config.
* Set PasswordAuthentication to yes.
* Reload SSH service: service sshd reload.
### Generate SSH Key for ansadmin User

* Switch to the ansadmin user.
* Generate an SSH key pair using ssh-keygen.
* Navigate to the .ssh directory and find the public key (id_rsa.pub).

## Ansible Installation
### Install Ansible
* Log in as ansadmin and switch to the root user.
* Install Ansible using Amazon Linux extras: amazon-linux-extras install ansible2.
* Verify Ansible installation: ansible --version.

## Integrating Ansible with Jenkins
### Configure Jenkins

* In the Jenkins dashboard, navigate to "Manage Jenkins" > "System."
* Under "Publish over SSH," add a new SSH server configuration:
    * Name: Ansible-Server
    * Password: P@ss12345
    * Hostname: [IP of your Ansible server on AWS]
## Create Docker Directory on Ansible Server

* Connect to the Ansible server.
* Create a directory for Docker: sudo mkdir /opt/Docker.
* Assign ownership to ansadmin: sudo chown ansadmin:ansadmin /opt/Docker.
## Set Up Jenkins Job for Copying Artifacts

* Create a new Jenkins job: "Copy_Artifacts_onto_Ansible."
* Choose "Maven Project" and configure Git repository.
* In post-build steps, use "Send build artifacts over SSH":
    * Choose the Ansible server configured earlier.
    * Source files: webapp/target/*.war
    * Remote directory: /opt/Docker
## Run Jenkins Job

* Trigger the job by clicking "Build Now."
* Verify on the Ansible server that the artifact (webapp.war) is copied to the Docker directory.

# Step 5.Installing and Configuring Docker on Ansible Server
 Docker allows you to run, build, and manage applications in containers, making deployment and management efficient and consistent.
## Docker Installation and Configuration
### Install Docker

* On your Ansible Server, execute: sudo yum install docker
* Ensure the ansadmin user is part of the Docker group:
```bash
sudo usermod -aG docker ansadmin
```
* To ensure proper permissions, start the Docker service: sudo service docker start
* Reboot the server to apply changes.
### Creating a Dockerfile

* Log in as ansadmin and ensure Docker is running: sudo systemctl start docker
* Navigate to the Docker directory: cd /opt/Docker/
* Create a Dockerfile: nano Dockerfile
* Add the following lines to the Dockerfile:
```bash
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
COPY ./*.war /usr/local/tomcat/webapps
```
* Save and exit.
### Creating Ansible Playbook for Docker Image

* In your Ansible Server's /etc/ansible/hosts file, specify the IP address of the server.
```bash
[ansible]
172.31.26.249
```
* Copy your SSH key to the localhost: ssh-copy-id 172.31.26.249
* Create a playbook: nano regapp.yml
* Add the following content:
```YAML
---
- hosts: ansible
  tasks:
  - name: create docker image
    command: docker build -t regapp:latest .
    args:
     chdir: /opt/Docker
  - name: create tag to push image onto Docker Hub
    command: docker tag regapp:latest devops810/regapp:latest
  - name: push Docker image to Docker Hub
    command: docker push devops810/regapp:latest
```
* Save and exit.
## Running the Ansible Playbook

    * Before running the playbook, log in to Docker Hub: docker login
    * Check the playbook's syntax: ansible-playbook regapp.yml --check
    * If syntax is okay, execute the playbook: ansible-playbook regapp.yml
## Verifying Docker Images

* Verify the images using: docker images
* You'll see the tomcat image pulled from Docker Hub and your custom regapp image created with the playbook.

# Step 6.Updating Jenkins Job to Use the Ansible Playbook
This section explains how to update your Jenkins job to execute an Ansible playbook. It guides you through configuring Jenkins to trigger Ansible automation for your deployments.
## Updating Jenkins Job
### Access Jenkins Dashboard

* Go to your Jenkins dashboard.
* Choose the project "Copy_Artifacts_onto_Ansible" (Maven project) and click "Configure."
### Configuring Post-build Action

* Scroll down to "Post-build Actions."
* Under "Send build artifacts over SSH," in the "Transfers" section, find "Exec command."
* Add the command to run the Ansible playbook:
```bash
ansible-playbook /opt/Docker/regapp.yml
```
* Click "Apply" and "Save."
# Setting Up Bootstrap Server for eksctl
This section explains how to set up a Bootstrap Server for eksctl (Amazon EKS command-line tool). It covers installing required tools, such as kubectl and eksctl, as well as creating and attaching an IAM role to the EC2 instance.

## Installing kubectl and eksctl
### Access the Bootstrap Server

* Launch an AWS instance named "EKS_Bootstrap_Server."
* Connect to the instance via SSH using the key.pem file.
### Change Hostname and Reboot

* Log in as the root user.
* Change the hostname: sudo vi /etc/hostname (set to "EKS_Bootstrap_Server").
* Reboot the server to apply the changes: sudo init 6.
### Install kubectl

* Become the root user: sudo su
* Download kubectl: curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl
* Provide executable permission: chmod +x ./kubectl
* Move kubectl to /usr/local/bin: mv kubectl /usr/local/bin
### Check kubectl Version

* Verify the installed version: kubectl version --output=yaml
### Install eksctl

* Download eksctl: curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
* Move eksctl to /usr/local/bin: mv /tmp/eksctl /usr/local/bin
## Create and Attach IAM Role to EC2 Instance
### Create IAM Role

* In the AWS Console, navigate to IAM > Roles > Create Role.
* Choose the trusted entity as AWS service, select EC2, and proceed.
* Attach necessary policies (e.g., AmazonEC2FullAccess, AWSCloudFormationFullAccess, IAMFullAccess).
### Assign Permissions

* Select additional custom policies as needed.
* For this guide, also attach the AdministratorAccess policy (note: this is not recommended in production).
* Name the role (e.g., eksctl_role) and create it.
### Attach Role to EC2 Instance

* Go to the EC2 instance's Actions > Security > Modify IAM role.
* Select the newly created role (e.g., eksctl_role) and update the IAM role.

# Step 7.Setting Up Kubernetes using eksctl
This section explains the process of setting up Kubernetes using eksctl. It guides you through creating a Kubernetes cluster and deploying applications within it.
## Creating a Kubernetes Cluster
### Access the EKS_Bootstrap_Server Terminal
* Log in to the EKS_Bootstrap_Server terminal.
* Ensure you're in the root directory.
### Create Kubernetes Cluster
* Use the command to create a Kubernetes cluster:
```bash
eksctl create cluster --name virtualonebox-cluster \
>  --region us-east-1 \
>  --node-type t2.micro \
>  enter
```
* Cluster creation may take around 20 to 25 minutes.
* Confirm cluster creation by running: kubectl get nodes
### Kubernetes Cluster Status
* Running kubectl get nodes displays the nodes in your cluster.
* This confirms that your Kubernetes cluster is ready.
## Creating Deployment Manifest File
* Generate the deployment manifest file: nano regapp-deployment.yml
* Add the following content to the file:
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: virtualonebox-regapp
  labels:
    app: regapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: regapp
  template:
    metadata:
      labels:
        app: regapp
    spec:
      containers:
      - name: regapp
        image: devops810/regapp
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
```
* Save the file.
## Create Service Manifest File
* Create the service manifest file: nano regapp-service.yml
* Add the following content to the file:
```bash
apiVersion: v1
kind: Service
metadata:
  name: virtualonebox-service
  labels:
    app: regapp
spec:
  selector:
    app: regapp
  ports:
    - port: 8080
      targetPort: 8080
  type: LoadBalancer
```
* Save the file.