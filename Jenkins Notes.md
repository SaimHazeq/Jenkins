# Jenkins Notes & CI/CD Guide

## 📌 Introduction

[Jenkins](https://www.jenkins.io/) is a free and open-source automation server mainly used for Continuous Integration (CI) and Continuous Delivery/Deployment (CD).

---

# 🚀 What is CI/CD?

## Continuous Integration (CI)

Continuous Integration means automatically building and testing code whenever developers push changes.

### Before CI
- Manual process
- Time consuming
- Higher chances of errors

### After CI
- Automated process
- Faster delivery
- Reduced manual effort

### Example

Day 1 → 100 lines → Build + Test  
Day 2 → 200 lines → Build + Test  
Day 3 → 300 lines → Build + Test

---

## Continuous Delivery / Deployment (CD)

CD automates deployment to environments.

### Environments

| Environment | Purpose |
|---|---|
| DEV | Developers |
| QA | Testers |
| UAT | Clients |
| PROD | End Users |

---

# 🔄 Pipeline

A pipeline is a step-by-step automated workflow.

```text
Code → Compile → Test → Artifact → Deployment
```

---

# ⚙️ Jenkins Overview

## Features

- Free & Open Source
- Written in Java
- Platform Independent
- Plugin Support
- Community Support
- Automates SDLC

## History

- Originally called Hudson
- Created by Kohsuke Kawaguchi
- Later renamed to Jenkins

## Default Details

| Item | Value |
|---|---|
| Port | 8080 |
| Default Path | `/var/lib/jenkins` |
| Java Versions | Java 11 / 17 |

---

# 🔁 Jenkins Alternatives

- Bamboo
- GitLab CI
- CircleCI
- GoCD
- Semaphore
- Harness
- ArgoCD
- AWS CodePipeline
- Azure Pipelines

---

# ☁️ Jenkins Setup on AWS EC2

## Step 1: Install Git, Java & Maven

```bash
yum install git java-1.8.0-openjdk maven -y
```

---

## Step 2: Add Jenkins Repository

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import \
https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

---

## Step 3: Install Java 11 & Jenkins

```bash
amazon-linux-extras install java-openjdk11 -y

yum install jenkins -y

update-alternatives --config java
```

---

## Step 4: Start Jenkins

```bash
systemctl start jenkins.service

systemctl status jenkins.service
```

---

# 🌐 Access Jenkins

Open in browser:

```text
http://<PUBLIC_IP>:8080
```

Get admin password:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

# 🧩 Jenkins Jobs

A Jenkins Job is used to perform tasks.

## Create a Freestyle Job

```text
New Item → Enter Name → Freestyle Project
```

### Configure SCM

```text
SCM → Git → Repository URL
```

### Build Step

```bash
mvn clean package
```

### Run Job

```text
Build Now
```

---

# 📁 Workspace

Where your job output is going to be stored.                                 
Default workspace path:

```bash
/var/lib/jenkins/workspace
```

---

# 🖥️ Jenkins Setup Script

## jenkins.sh

```bash
#!/bin/bash

yum install git java-1.8.0-openjdk maven -y

sudo wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import \
https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

amazon-linux-extras install java-openjdk11 -y

yum install jenkins -y

systemctl start jenkins.service
```

Run script:

```bash
sh jenkins.sh
```

---

# 🧮 Variables in Jenkins

It is used to store values that are going to change frequently.

## Types of Variables

## 1. User Defined Variables                                                  
These are defined by user                                                     
## 2. Jenkins Environment Variables
These are defined by Jenkins itself.                                            
These variables can be change from build to build.                               
These variables will be on Upper case.                                           
These variables can be defined only once.

---
## User Defined Variables
## i) Local Variable

```text
New Item → Name: XYZ → Freestyle Project → OK → Build → Execute Shell
```
```bash
name=saim

echo "Hi my name is $name"
```

---

## ii) Global Variable

```text
Dashboard → Manage Jenkins → System → Global Properties → Environment Variables → ADD:NAMW:name VALUE:saim → Save.
```
## Note:                                                                      
- While defining variables SPACE will not be given.                             
- Local variables will be high priority.

## Limitations:                                                             
- Some values can't be defined by user because these values will change build by build. 

# 🌍 Jenkins Environment Variables

Examples:

```bash
echo $BUILD_NUMBER
echo $JOB_NAME
```

Show all environment variables:

```bash
printenv
```

---

# 🔧 Jenkins Admin Tasks

## Change Jenkins Port

Edit:

```bash
vim /usr/lib/systemd/system/jenkins.service
```

Change:

```text
8080 → 8090
```

Restart:

```bash
systemctl daemon-reload
systemctl restart jenkins.service
```

---

## Passwordless Login

Edit:

```bash
vim /var/lib/jenkins/config.xml
```

Change:

```text
true → false
```

Restart Jenkins:

```bash
systemctl restart jenkins.service
```

---

# 🧵 Parallel Builds & Executors

Jenkins will run the jobs sequentially (one by one).                            
If i want to run multiple builds at same time we can configure ⬇️

## Enable Concurrent Builds

```text
Job → Configure → Execute concurrent builds if necessary → Save
```

---

## Build Executors

```text
Manage Jenkins →
Built-in Node →
Configure →
Executors: 2-5
```

---

# ⏰ Cron Jobs in Jenkins

We can schedule the jobs that need to be run at particular intervals.            
Here we can use cron syntax.                                                    
Cron syntax has * * * * *                                                        
Each * is separated by space

Cron format:

```text
* * * * *
| | | | |
| | | | Day of Week
| | | Month
| | Date
| | Hour
| Minute
```

Example:

```text
30 18 14 9 0
```
```text
Create Job → Build Triggers → Build Periodically → Save
```

---
### ✋Limitation: 
- It will not check the code is changed or not.

# 🔍 Poll SCM
In PollScm we will set time limit for the job.                                  
If Developer commit the code it will wait until the time is done.               
In given time if we have any changes on code it will generate a build.           

Checks Git repository periodically.

```text
Create Job → Build Triggers → Poll SCM → * * * * * → Save
```

### ✋Limitations
- We need to wait for the time we set
- Will gets only latest commit

---

# 🔗 GitHub Webhook

It will trigger build the moment we change the code.                            
Here we need not to wait for the build.                                         
Triggers build instantly after code push.

## Setup

```text
GitHub Repo → Settings → Webhooks → Add Webhook
→ Payload URL (Jenkins URL) → Content Type
→ Application/json → Add
```
```text
Create Job →
Build Triggers:GitHub hook trigger for GITScm polling → Save
```

Payload URL:

```text
http://<JENKINS_IP>:8080/github-webhook/
```

Enable:

```text
GitHub hook trigger for GITScm polling
```

---

# 🚦 Throttle Builds

To Restrict number of builds in specific time or intervals.                     
If we don't restrict due to immediate builds Jenkins might crashdown.             
By default Jenkins will not do concurrent builds.                                
We need to enable this option in configuration.

```text
Execute concurrent builds if necessary → ✅Tick it 
```

```text
Create Job → Configure → Throttle Builds → Number of Builds →
Time Period → Save
```

---

## 👀Note:
If we stop server then services inside server also going to stop.
```
chkconfig jenkins on
```
The above command will restart the jenkins service all the time

# 📦 Jenkins Pipeline

Step By Step Execution Of A Process.                                            
Series Of Events Interlinked With Each Other.

A Jenkins Pipeline automates software delivery.

```text
Code → Build → Test → Artifact → Deployment
```
If Pipeline fails on Stage-1 it won't go for the Stage-2

---

# 🧱 Types of Pipelines

## 1. Declarative Pipeline
## 2. Scripted Pipeline

To write the pipeline we use DSL.                                               
We use Groovy Script for Jenkins Pipeline.                                      
It consists of blocks that include stages.                                       
It include ( ) & { } braces.

---

# 📝 Declarative Pipeline Syntax

## Shortcut

```text
P → Pipeline
A → Agent
S → Stages
S → Stage
S → Steps
```

---

# 🔹 Single Stage Pipeline

```groovy
pipeline {
    agent any

    stages {
        stage('one') {
            steps {
                sh 'touch file1'
            }
        }
    }
}
```

---

# 🔹 Multi Stage Pipeline

```groovy
pipeline {
    agent any

    stages {

        stage('one') {
            steps {
                sh 'lsblk'
            }
        }

        stage('two') {
            steps {
                sh 'lscpu'
            }
        }

        stage('three') {
            steps {
                sh 'lsmem'
            }
        }
    }
}
```

---

# 🚀 CI Pipeline Example

```groovy
pipeline {
    agent any

    stages {

        stage('checkout') {
            steps {
                git 'https://github.com/Repository_URL.git'
            }
        }

        stage('build') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('artifact') {
            steps {
                sh 'mvn package'
            }
        }
    }
}
```

---

# 🧩 Pipeline as Code (PAAC)

```groovy
pipeline {
    agent any

    stages {

        stage('checkout') {
            steps {
                git 'https://github.com/Repository_URL.git'

                sh 'mvn compile'
                sh 'mvn test'
                sh 'mvn package'
            }
        }
    }
}
```

---

# 🖥️ Single Shell Pipeline

```groovy
pipeline {
    agent any

    stages {

        stage('one') {
            steps {

                sh '''
                mvn compile
                mvn test
                mvn package
                '''
            }
        }
    }
}
```

---

# ✅ Input Parameters

```groovy
stage('deploy') {

    input {
        message "Is your input correct?"
        ok "yes"
    }

    steps {
        echo "Code deployed"
    }
}
```
### Note: 
- In real time providing manula approval is best practices for Jenkins Pipelines.
- If we have syntax issue none of the stages will execute.
---

# ⚖️ Scripted vs Declarative Pipeline

| Scripted | Declarative |
|---|---|
| Complex | Simple |
| Uses `node` | Uses `pipeline` |
| More flexible | Easier to read |
| No stages required | Uses stages |

---

# 🧠 Jenkins Master & Agent

## Why Agents?
- It is used to distribute the builds
- It reduce the load on Jenkins server
- Reduce load on Jenkins master
- Run parallel builds
- Distributed execution

Communication uses:

```text
SSH
```

---

# ⚙️ Configure Jenkins Agent

## Install Java

```bash
amazon-linux-extras install java-openjdk11 -y
```

---

## Create Agent Node

```text
Dashboard → Job →
Manage Jenkins → 
Nodes & Clouds →
New Node →
Nodename: XYZ →
Permanent Agent → Save
```

---

## Agent Configuration

| Setting | Value |
|---|---|
| Executors | 3 |
| Remote Root Directory | `/tmp` |
| Labels | App Name |
| Usage | Use this node as much as possible |

---

# 🏷️ Run Job on Specific Agent

```groovy
pipeline {

    agent {
        label 'saim'
    }

    stages {

        stage('code') {
            steps {
                git 'https://github.com/Repository_URL.git'
            }
        }

        stage('build') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('artifact') {
            steps {
                sh 'mvn package'
            }
        }
    }
}
```

---

# 📚 Useful Linux Commands

## Find Files

```bash
find / -name file_name
```

Example:

```bash
find / -name jenkins.service
```

---

# ✅ Best Practices

- Use Webhooks instead of Poll SCM
- Use Pipelines as Code
- Enable manual approval before production deployment
- Use agents for distributed builds
- Restrict concurrent builds to avoid overload

---

# 📖 References

- https://www.jenkins.io/
- https://github.com/
- https://crontab-generator.org/

---

# 👨‍💻 Author

Prepared from Jenkins learning notes and CI/CD practice documentation.
