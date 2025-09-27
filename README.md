# Jenkins CI/CD Pipeline for Flask Application 🚀

# 💡 Project Overview

This repository demonstrates a Jenkins-based CI/CD pipeline for a basic Flask web app. The automated workflow includes:

- **Build** – Set up a Python virtual environment and install required packages.
- **Test** – Execute unit tests using pytest to ensure code quality.
- **Deploy** – Automatically deploy the Flask application to a staging server upon successful tests.
- **Notify** – Send email alerts to inform about the build and deployment status.



## 1) Create EC2 instance for Jenkins 

Open port 22 for `ssh`, 8080 for `Jenkins` and 5000 for `flask` app
<img width="1600" height="870" alt="image" src="https://github.com/user-attachments/assets/e5a78b59-095e-47ba-937a-c3a67ba2fde4" />

## 2) Install Jenkins on EC2

create a shell file install_jenkins.sh with all commands to install and start Jenkins and execute with following commands

<img width="1267" height="296" alt="Screenshot 2025-09-27 200626" src="https://github.com/user-attachments/assets/09ea38b5-b7b7-45fd-b696-f181a9f606ab" />
<img width="1284" height="925" alt="Screenshot 2025-09-27 200553" src="https://github.com/user-attachments/assets/5f868ed8-0a1b-49e9-a231-357c923150b1" />



Access Jenkins on http://EC2IP:8080

<img width="1906" height="969" alt="Screenshot 2025-09-27 211958" src="https://github.com/user-attachments/assets/4d5c676c-5e16-40d9-b4f7-c7157f37e7bc" />


## 3) Create Jenkinsfile in repo

<img width="1375" height="915" alt="Screenshot 2025-09-27 204727" src="https://github.com/user-attachments/assets/f1afa284-36e8-4d04-9ca7-8f7fffdf6442" />


## 4) Create a webhook

GitHub → Your Repo → Settings → Webhooks → Add Webhook:
Payload URL: http://EC2IP:8080//github-webhook/
Content Type: application/json.
Event: "Just Push events"

<img width="1057" height="920" alt="Screenshot 2025-09-27 201655" src="https://github.com/user-attachments/assets/997ad3ff-dcd3-4abf-9c05-bc751f6ab922" />


## 5) Create a Job

Create a New Job:
Go to Jenkins dashboard → New Item
Choose: Pipeline
Select github hook trigger for GITscm polling
Scroll to Pipeline → Definition → Pipeline script from SCM
SCM: Git
Repo URL: https://github.com/aishp411/FlaskTest.git
Branch: main
Script Path: Jenkinsfile


<img width="1920" height="1080" alt="Screenshot 2025-09-24 173631" src="https://github.com/user-attachments/assets/742922c1-567e-4b24-ba27-c4004c4a4573" />

## 6) Jenkins Email Notification Configuration
<img width="1898" height="1019" alt="Screenshot 2025-09-25 212833" src="https://github.com/user-attachments/assets/5597c078-9723-4d95-9d90-ee20da3d4095" />



## 7) Trigger pipeline by pushing changes

<img width="1913" height="975" alt="Screenshot 2025-09-24 183148" src="https://github.com/user-attachments/assets/16c8f7eb-dc53-481d-8d54-d8cbb401f613" />


## 8) Check pipeline in Jenkins 


<img width="1894" height="971" alt="Screenshot 2025-09-27 205108" src="https://github.com/user-attachments/assets/4dc0b813-26c4-4756-a49d-5acdc3a56547" />

## 9) Check Deployment in EC2 


<img width="1397" height="829" alt="Screenshot 2025-09-27 204711" src="https://github.com/user-attachments/assets/e5fa6403-bd3a-467c-b003-e6cf29a2c6a3" />

## 10) Email Notification 

<img width="1092" height="679" alt="Screenshot 2025-09-27 214036" src="https://github.com/user-attachments/assets/bd4aba83-fd21-4b4a-8018-3cdba594f366" />

