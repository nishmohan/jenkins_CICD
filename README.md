# Flask Jenkins CI/CD

Set up a Jenkins pipeline that automates the testing and deployment of a simple Python web application.
Project Repository: https://github.com/nishmohan/jenkins_CICD

## Objective

- Install Jenkins on a virtual machine or use a cloud-based Jenkins service.
- Configure Jenkins with Python and any necessary libraries.
- Configure the pipeline to trigger a new build whenever changes are pushed to the main branch of the repository.
- Set up a notification system to alert via email when the build process fails or succeeds.
- 
## Setup EC2 Instances

Log in to AWS Management Console. Navigate to EC2 Dashboard and click "Launch Instance". Choose Ubuntu Server as the OS, instance type and key pair for SSH access. Configure Security Group to allow ports 22 (SSH), 80 (HTTP), 443 (HTTPS),8080, and 3000. 

## Install Jenkins
Connect with EC2 instance then following commands:
  ```
  sudo apt update 
  sudo apt install openjdk-11-jdk -y 
  wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key |
  sudo apt-key add - sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ >
  /etc/apt/sources.list.d/jenkins.list' 
  sudo apt update 
  sudo apt install jenkins -y
  ```
  -	After installation start the Jenkins:
  ```
  sudo systemctl start jenkins
  sudo systemctl enable Jenkins
  ```
  -	After that I access Jenkins using URL: http://<your_vm_ip>:8080
  -	I did complete setup with initial admin password and create user name and password.
  -	After login to Jenkins configure the following plugins:
      o	Git
      o	Pipeline
      o	Email Extension
      o	GitHub Integration
<img width="940" height="474" alt="image" src="https://github.com/user-attachments/assets/b68b18fb-e146-4016-958e-3d9a8ff86daa" />

-	Install Python and pip on the server:
  ``` sudo apt install python3 python3-pip -y pip3 install flask pytest ```
## Setup SMTP to Jenkins
-	Under Manage Jenkins > Configure System, in Extended E-mail Notification:
 ```   SMTP Server: smtp.gmail.com
    Use SSL:  (check it)
    SMTP Port: 465
    SMTP Authentication:
    User Name: your-email@gmail.com
    Password: your-app-password (⚠ See note below)
    Reply-To Address: your-email@gmail.com
    Default Subject: Build Notification
    Default Content: $DEFAULT_CONTENT
    Charset: UTF-8
    Important: For Gmail, you cannot use your account password. You must use an
    App Password (enabled via Google Account > Security > App Passwords).
    So I setup App Password for it:
    Go to https://myaccount.google.com
    Enable 2-Step Verification
    Go to Security > App Passwords
    Generate a new password for "Jenkins" app
    Use this password in Jenkins (instead of your real Gmail password)
```
## Setup Jenkins Pipeline
  -	I setup pipeline as below screen shots:
    <img width="775" height="1453" alt="image" src="https://github.com/user-attachments/assets/a2d9b436-4634-41ef-824f-dc29eb51f449" />
## Setup Webhook at Github Repository
  -	Repo -> Setting-> Webhook then created a webhook to build and deploy the code using Jenkinfile. More reference check below screenshot:
    <img width="940" height="836" alt="image" src="https://github.com/user-attachments/assets/34c8391a-238f-4d79-b157-f93c38b90875" />

## Pipeline Stages:
<img width="574" height="352" alt="image" src="https://github.com/user-attachments/assets/95e3baee-0957-424b-9779-cba0dd109f6d" />

<img width="799" height="689" alt="image" src="https://github.com/user-attachments/assets/5c52109f-20cd-4f01-a439-6f2206ca3d90" />

## Webhook Request and Response
<img width="940" height="515" alt="image" src="https://github.com/user-attachments/assets/5e9019a8-ac2a-44b1-9576-d21f6316aac7" />

<img width="940" height="316" alt="image" src="https://github.com/user-attachments/assets/57f07918-bbe3-4ea1-9ef5-f227ef68db43" />

## Jenkins Pipeline Running
<img width="939" height="401" alt="image" src="https://github.com/user-attachments/assets/4015ee7f-6b15-4f41-961e-3b0efdbdd801" />
<img width="940" height="1059" alt="image" src="https://github.com/user-attachments/assets/737dbf2a-3a2e-44de-84ce-7c2b12573294" />

## Email Notification
<img width="940" height="411" alt="image" src="https://github.com/user-attachments/assets/d4e87b9f-a18e-442a-888d-b9d3e1a06cda" />












