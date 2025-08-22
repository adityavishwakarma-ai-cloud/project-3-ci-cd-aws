# ⚙️ Project 3: GitLab CI/CD Pipeline for Django/Node App on AWS EC2

## 📌 Project Overview

This project demonstrates the **automation of application deployment** using GitLab CI/CD pipelines.
It covers:

* Containerizing a Django or Node.js application using Docker
* Automated building, testing, and deployment on **AWS EC2 instances**
* Continuous integration and continuous deployment for reliable production delivery

The goal is to **reduce manual deployments**, ensure **faster release cycles**, and maintain **code quality**.

---

## 🛠️ Tech Stack / Tools Required

* **GitLab** (CI/CD Pipelines)
* **Docker & Docker Compose** (Containerization)
* **AWS EC2** (Hosting environment)
* **Node.js / Django** (Application stack)
* **Git & GitHub / GitLab** (Version control)

---

## 📂 Functional Requirements

* Automate **build, test, and deployment** for web application.
* Deploy application on **AWS EC2 instance** using CI/CD pipeline.
* Run unit tests automatically on each commit.
* Notify team on pipeline success or failure.

---

## 📂 Non-Functional Requirements

* Ensure **high availability** and fault tolerance.
* Pipeline should be **secure**, using AWS credentials securely.
* Deployment should be **repeatable** and consistent across environments.
* Reduce **manual intervention** in deployment processes.

---

## 🔄 Project Flow

1. **Developer commits code** to GitLab repository.
2. **GitLab CI/CD pipeline triggers** automatically.

   * Build Docker image of the application.
   * Run automated unit tests.
   * Push Docker image to **Docker registry** (optional).
3. **Deploy pipeline** executes:

   * SSH into **AWS EC2 instance**.
   * Pull or build Docker image.
   * Run container using Docker Compose.
4. **Application is live** on EC2 public IP or domain.
5. **Pipeline logs** and status are available in GitLab CI/CD dashboard.

---

## 🚀 How to Run the Project

### 1️⃣ GitLab Pipeline Configuration (`.gitlab-ci.yml`)

```yaml
stages:
  - build
  - test
  - deploy

variables:
  DOCKER_HOST: tcp://docker:2375/
  DOCKER_DRIVER: overlay2

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t myapp:latest .
    - docker images

test:
  stage: test
  image: node:18
  script:
    - npm install
    - npm test

deploy:
  stage: deploy
  image: alpine:latest
  script:
    - apk add --no-cache openssh-client
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - ssh-keyscan -H $EC2_HOST >> ~/.ssh/known_hosts
    - ssh $EC2_USER@$EC2_HOST "cd /home/ubuntu/app && docker-compose pull && docker-compose up -d --build"
```

> Replace `$SSH_PRIVATE_KEY`, `$EC2_HOST`, and `$EC2_USER` with your AWS EC2 credentials.

---

## 📖 Folder Structure

```
project-3-ci-cd-aws/
│-- README.md                # Documentation
│-- .gitlab-ci.yml           # GitLab CI/CD pipeline file
│-- docker-compose.yml       # Container orchestration
│-- web/                     # Application source code
│   ├── Dockerfile
│   ├── app.js or manage.py
│   └── package.json / requirements.txt
│-- db/                      # Database container (optional)
│-- scripts/                 # Optional deployment scripts
│-- .gitignore               # Ignore unnecessary files
```

---

## 📝 Resume Highlights

**Objective**
Implemented CI/CD pipelines to automate deployment of web applications on AWS, ensuring fast and reliable delivery.

**Key Achievements**

* Designed **GitLab CI/CD pipeline** for automated build, test, and deploy.
* Deployed **Django/Node.js apps** on AWS EC2 using Docker Compose.
* Integrated **unit tests and security checks** in CI/CD process.

**Impact**
✅ Reduced manual deployment effort by **90%**.
✅ Increased deployment reliability and repeatability.
✅ Enabled **faster feature delivery** with automated pipelines.

---

## 🔗 Repository Link

*(Replace with your repo link after upload)*

```md
https://github.com/<your-username>/project-3-ci-cd-aws
```

