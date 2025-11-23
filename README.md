# Jenkins-Learning

# Jenkins – Lecture 1 Notes

## 1. What is Jenkins?
Jenkins is an **open-source automation server** used for:
- Continuous Integration (CI)
- Continuous Delivery / Deployment (CD)
- Automating build, test, and deployment pipelines

It pulls code, builds, tests, creates artifacts (Docker images, JAR files), deploys applications, and sends notifications.

---

## 1.1 Does Jenkins Require a Dedicated Server?
Not mandatory, but **recommended** because Jenkins needs:
- Stable uptime  
- Continuous execution  
- Plugin and job management  
- Reliable environment  

Jenkins can run on:
- Local machine  
- Virtual machine  
- Cloud server  
- Docker container  
- Kubernetes pod  

---

## 1.2 Why is Jenkins Called a Master (Controller)?
Jenkins uses a **Master/Agent architecture**:

Jenkins Master (Controller)
|
| Schedules & assigns jobs
v
Jenkins Agents (Workers)


- **Master / Controller**: Manages jobs, scheduling, UI, plugins  
- **Agents / Nodes**: Execute builds, tests, deployments  

---

## 2. Jenkins Flow Diagram

Developer → Push Code to GitHub → Jenkins Master (triggered via webhook/SCM polling) → Build Agent → Test Agent → Build Artifacts (e.g., Docker image) → Deploy to Server


---

## 3. What is a Jenkins Job?
A **Jenkins Job** is a defined task that Jenkins executes.

Types of Jobs:
- Freestyle Job  
- Pipeline Job (Jenkinsfile)  
- Multi-Branch Pipeline  
- Folder/Organized jobs  

A job may:
- Pull code  
- Build (Maven, npm, Gradle, Docker)  
- Run tests  
- Deploy applications  
- Send notifications  

---

## 4. Continuous Delivery vs Continuous Deployment

| Topic | Continuous Delivery | Continuous Deployment |
|--------|-----------------------|--------------------------|
| Deployment | Manual approval required | Fully automated |
| Production Release | Human-triggered | Automatic |
| Risk | Lower | Higher |
| Summary | Auto-build & test, manual deploy | No manual step, auto deploy |

**Continuous Delivery:** Deployment is ready but requires human approval.  
**Continuous Deployment:** Each successful build directly goes to production automatically.

---

## 5. Continuous Delivery Project (Todo-App using Freestyle Job)

### Steps performed:
1. Pull code from GitHub  
2. Stop running containers  
3. Rebuild and restart services using Docker Compose  
4. Send email for unstable/failed builds  

### Commands used:
```bash
docker-compose down
docker-compose up -d --build

Why Docker Compose?

* Manages multiple containers (frontend, backend, DB)
* Easy orchestration
* Good for staging/production CD workflows

6. Email Notifications Setup

Using the Email Extension Plugin:

* Configure SMTP (sender email)
* Add recipient list
* Add "Editable Email Notification" in Post-Build Actions
* Trigger emails on:
  * Failed builds
  * Unstable builds
  * Successful recovery

7. Continuous Deployment Project (Todo-App using Freestyle Job)

In Continuous Deployment Everything is fully automated and No manual intervention is required
Flow: Git push → Jenkins Job → Build → docker-compose down/up → Auto deploy to production

8. Both Projects Used Freestyle Jobs

* Continuous Delivery (CD) → Freestyle
* Continuous Deployment (CD) → Freestyle
* Pipelines were not used in these two projects

9. Jenkins Pipeline Overview

A Jenkins Pipeline is a code-based CI/CD process written in a Jenkinsfile.

Benefits:
* Version controlled
* Multi-stage workflows
* Parallel execution
* Better error handling
* Reproducible & maintainable

Sample Declarative Pipeline

pipeline {
    agent any

    stages {
        stage('Pull Code') {
            steps {
                git 'https://github.com/user/todo-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }
}

| Concept               | Explanation                           |
| --------------------- | ------------------------------------- |
| Jenkins               | Automation server for CI/CD           |
| Dedicated Server      | Optional but recommended              |
| Master/Agent          | Master controls, agents build         |
| Job                   | Unit of work (build/test/deploy)      |
| Continuous Delivery   | Auto build/test, manual deploy        |
| Continuous Deployment | Fully automated deploy                |
| CD Project            | docker-compose down/up + email alerts |
| Deployment Project    | Fully automated deployment            |
| Pipeline              | Jenkinsfile-based CI/CD process       |

