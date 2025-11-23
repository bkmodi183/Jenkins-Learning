# Jenkins Learning -- README

## 📘 Overview

This README contains well‑structured notes of Jenkins learning.\
It covers CI/CD concepts, Jenkins Master-Agent architecture, Freestyle
jobs, Pipelines, and practical projects.

------------------------------------------------------------------------

# 🚀 Jenkins -- Lecture 1 Summary

## 1. What is Jenkins?

Jenkins is an **open-source automation server** used for: - Continuous
Integration (CI) - Continuous Delivery / Deployment (CD) - Automating
build, test & deployment workflows

It can: - Pull code\
- Build applications\
- Test applications\
- Create artifacts (JAR, Docker image, etc.)\
- Deploy apps\
- Send notifications

------------------------------------------------------------------------

## 1.1 Does Jenkins Require a Dedicated Server?

Not mandatory but **recommended** for: - Continuous uptime\
- Better reliability\
- Plugin & job management

Jenkins can run on: - Local machine\
- VM\
- Cloud server\
- Docker container\
- Kubernetes pod

------------------------------------------------------------------------

## 1.2 Why Jenkins is Called Master/Controller?

Jenkins uses **Master-Agent Architecture**.

**Master/Controller** - Schedules jobs\
- Manages UI & plugins\
- Controls workflow

**Agents/Nodes** - Execute builds, tests, deployments

------------------------------------------------------------------------

## 2. Jenkins Flow

Developer → GitHub → Jenkins Master → Build Agent → Test Agent →
Artifact → Deployment

------------------------------------------------------------------------

## 3. What is a Jenkins Job?

A Jenkins job is a **unit of work**, like: - Pulling code\
- Building (Maven, npm, Docker)\
- Testing\
- Deployment\
- Sending notifications

Types: - Freestyle\
- Pipeline\
- Multi-branch pipeline\
- Folder jobs

------------------------------------------------------------------------

## 4. Continuous Delivery vs Continuous Deployment

  Topic                Continuous Delivery              Continuous Deployment
  -------------------- -------------------------------- ------------------------
  Deployment           Manual approval                  Fully automated
  Production Release   Human-triggered                  Auto-triggered
  Risk                 Lower                            Higher
  Summary              Auto-build/test, manual deploy   Fully automated deploy

------------------------------------------------------------------------

## 5. Continuous Delivery Project (Todo App -- Freestyle)

Steps: 1. Pull GitHub code\
2. Stop running containers\
3. Rebuild & restart using Docker Compose\
4. Send email alerts

Commands:

``` bash
docker-compose down
docker-compose up -d --build
```

### Why Docker Compose?

-   Multi-container orchestration\
-   Easy staging/production setup

------------------------------------------------------------------------

## 6. Email Notifications

Using Email Extension Plugin: - Configure SMTP\
- Add recipients\
- Add Editable Email Notification\
- Trigger on failed/unstable builds

------------------------------------------------------------------------

## 7. Continuous Deployment Project (Todo App -- Freestyle)

Fully automated deployment: Git Push → Jenkins → Build → Docker Compose
→ Deploy automatically

Both projects used **Freestyle Jobs**.

------------------------------------------------------------------------

## 8. Jenkins Pipeline Overview

Pipelines use Jenkinsfile (Groovy syntax).

### Benefits:

-   Version-controlled\
-   Multi-stage\
-   Parallel execution\
-   More reliable & maintainable

### Sample Declarative Pipeline:

``` groovy
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
```

------------------------------------------------------------------------

# 🧩 Jenkins -- Lecture 2 Summary

## 1. Declarative Pipeline Stages Created

-   code\
-   build\
-   push\
-   test\
-   deploy

**CI** = code + build + test\
**CD** = push + deploy

------------------------------------------------------------------------

## 2. Pipelines use Groovy Syntax

Inside:

``` groovy
pipeline { }
```

------------------------------------------------------------------------

## 3. What is `agent any`?

Jenkins selects any available agent for the pipeline.

------------------------------------------------------------------------

## 4. Running Jobs on Agents

Before running jobs: - Add agent to Jenkins via SSH\
- Install Java on agents\
- Install Docker on agents\
- Add user to Docker group

------------------------------------------------------------------------

## 5. Steps to Add an Agent

1.  Generate SSH keys on master\
2.  Copy public key → agent's `authorized_keys`\
3.  Add node in Jenkins\
4.  Add private key via SSH credentials\
5.  Install Java on agent\
6.  Agent comes online

------------------------------------------------------------------------

## 6. Labels

Each agent can have a **label**, used to run jobs on specific nodes.

Freestyle: choose label in settings\
Pipeline:

``` groovy
agent { label 'agent-1' }
```

------------------------------------------------------------------------

## 7. Freestyle Job Example

Django Todo App running on **agent-1**.

------------------------------------------------------------------------

## 8. Freestyle vs Declarative Pipeline

### Freestyle

-   Beginner-friendly\
-   GUI-based\
-   Simple tasks\
-   Quick automation

### Pipeline (Jenkinsfile)

-   Production-grade\
-   Multi-stage workflows\
-   Version-controlled\
-   Team friendly\
-   Best for CI/CD

------------------------------------------------------------------------

# 🌟 Conclusion

This README summarizes: - Jenkins CI/CD concepts\
- Master-Agent architecture\
- Freestyle jobs\
- Pipelines\
- Practical CD & deployment workflows\
- Agent setup\
- Pipeline stages

Perfect for students and DevOps beginners practicing Jenkins.

------------------------------------------------------------------------
