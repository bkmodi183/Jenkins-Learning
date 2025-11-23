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

# Jenkins – Lecture 2 Notes

1. Created a **Declarative Pipeline** with stages:
   * code
   * build
   * push
   * test
   * deploy

2. In this pipeline:
   * **code, build, test** → Continuous Integration  
   * **push, deploy** → Continuous Deployment  
   Together these stages form a complete **CI/CD pipeline**.

3. The number of stages in a pipeline depends on the DevOps engineer.  
   Each stage has importance.  
   - If a stage fails (for example, the build stage), the Groovy script **does not move** to the next stage until the issue is fixed.  
   - This prevents broken code from being deployed.

4. CI/CD Declarative Pipelines are governed by **Groovy syntax** written inside the `pipeline { }` block of a Jenkinsfile.

5. **What is `agent any`?**  
   - It means the pipeline can run on **any available agent** connected to the Jenkins master.  
   - Jenkins automatically selects a suitable agent to run the job.

6. Jenkins allows running jobs on multiple servers, but before that:
   - You must add each agent/server to the Jenkins master via **SSH**.
   - Install **Java** on each agent because Jenkins uses Java APIs to communicate.
   - Without Java, an agent cannot connect or respond to master commands.

7. **Steps to add an Agent:**
   * Go to the `.ssh` folder on the master server.
   * Generate an SSH key using `ssh-keygen` and copy the **public key**.
   * Go to the `.ssh` folder on the agent server and paste the public key inside the `authorized_keys` file.
   * In Jenkins: Dashboard → Nodes → Add Node  
     - Choose SSH credentials  
     - Add the **private key** copied from the master
   * The agent may still show “offline” until **Java is installed on the agent**.  
     After installing Java, the agent will come online and can execute jobs.

8. Running jobs on agents:
   * For **Freestyle Jobs** → Selecting agents is done via job configuration (restrict execution to a label).  
   * For **Declarative Pipelines** → Agents are selected using `agent { label 'label-name' }`.

9. When setting up an agent, we assign an **Agent Label**.  
   - Multiple agents can share the same label.  
   - Jenkins can distribute jobs among agents with the same label.

10. Before running a job on an agent, ensure:
    * The agent is **online** and connected to Jenkins master  
    * **Java** is installed  
    * **Docker** is installed  
    * The user has permission to run Docker without `sudo`  
      - Add the user to the `docker` group  
      - Reboot the machine

11. Created a Freestyle Job to run the **django-todo-app** on **agent-1**.  
    - In the project configuration, select the option to run the job on the connected agent.

12. When to Use Jenkins Freestyle vs Declarative Pipeline:

    **Freestyle Jobs:**
    * Simple automation
    * Beginners
    * Quick tests
    * Small projects or single-step tasks
    * GUI-based configuration

    **Declarative Pipelines (Jenkinsfile):**
    * Complex CI/CD workflows
    * Multi-stage processes  
    * Version-controlled pipelines  
    * Reproducible and auditable builds  
    * Best for production deployments  
    * Required when working with teams and Git-driven automation
