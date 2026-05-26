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

## Types of Variables

1. User Defined Variables
2. Jenkins Environment Variables

---

## Local Variable

```bash
name=saim

echo "Hi my name is $name"
```

---

## Global Variable

```text
Dashboard → Manage Jenkins → System →
Global Properties → Environment Variables
```

---

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

## Enable Concurrent Builds

```text
Job → Configure →
Execute concurrent builds if necessary
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

---

# 🔍 Poll SCM

Checks Git repository periodically.

```text
Build Triggers → Poll SCM
```

Example:

```text
* * * * *
```

### Limitations

- Waits for polling interval
- Gets only latest commit

---

# 🔗 GitHub Webhook

Triggers build instantly after code push.

## Setup

```text
GitHub Repo →
Settings →
Webhooks →
Add Webhook
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

Restrict number of builds in specific intervals.

```text
Configure →
Throttle Builds
```

---

# 📦 Jenkins Pipeline

A Jenkins Pipeline automates software delivery.

```text
Code → Build → Test → Artifact → Deployment
```

---

# 🧱 Types of Pipelines

1. Declarative Pipeline
2. Scripted Pipeline

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
Dashboard →
Manage Jenkins →
Nodes & Clouds →
New Node
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
