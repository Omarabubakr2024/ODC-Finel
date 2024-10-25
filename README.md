# Orange Final CI/CD Project

## Overview
This project involves deploying a Node.js application from the Node.js GitHub repository to a local Kubernetes cluster. We utilize ArgoCD for continuous delivery and set up a Jenkins pipeline to automate the build and deployment process.

## Prerequisites
- Virtual Machine (VM) for Jenkins
- Docker installed on the Jenkins VM
- Another VM for Minikube and kubectl
- Minikube installed on the second VM
- ArgoCD installed on Minikube
- Jenkins installed on the first VM
- GitHub account
- Docker Hub account

## Part 1: Jenkins Setup and Dockerization
### Step 1: Fork the Repository
Fork the repository: [Node.js GitHub Repository](https://github.com/nodejs/nodejs.org.git)

### Step 2: Set Up Jenkins Pipeline
Create a `Jenkinsfile` in the root of your repository with the following content:

```groovy
pipeline {
    agent { label 'built-in' }
    tools {
        nodejs 'NodeJS'
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Omarabubakr2024/ODC-Finel'
            }
        }
        stage('Check Node.js Version') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Unit Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Docker Image') {
            steps {
                sh 'echo "done"'
            }
        }
        stage('Push The Image') {
            steps {
                sh 'echo "done"'
            }
        }
    }
}
```
## Part 2: Deploy the Node.js App to Kubernetes using ArgoCD

### Prerequisites
- ArgoCD installed on Minikube.
- A cluster created in ArgoCD.

### Steps to Deploy
1. **Add Repository:**  
   Add the repository link to the ArgoCD Settings > Repositories.

2. **Create Automation Application:**  
   Create an application named `mac-cd` with automatic sync enabled.

3. **Deployment Configuration:**  
   Create a `deployment.yaml` file with the following configuration:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: node-app-deployment
     namespace: webapp
     labels: 
       app: myapp
       type: nodejs
   spec:
     selector:
       matchLabels:
         app: myapp
     replicas: 1
     template:
       metadata:
         labels:
           app: myapp
       spec:
         containers:
           - name: nodejs
             image: omarbanna/orange
             ports:
               - containerPort: 3000
   ```
