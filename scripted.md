# POC For Java - Scriptive Jenkins Pipeline
| Author  | Created on | Version   | Last Edited On | Comment  | Reviewer |
|---------|------------|-----------|----------------|-------------------|---------------|
| Shubham | 28-04-25   |  version1| 28-04-25        | Internal Review    | Komal Jaiswal|
| Shubham | 28-04-25  |  version1|-   | L0  Review  | Gaurav Singla |
| Shubham | 28-04-25  |  version1| -     | L1  Review | Rahul Gupta |
| Shubham | 28-04-25   |  version1| -      | L2  Review  | Mahesh Kumar|

##  Table of Contents
1. [Jenkins Installations](#jenkins-installations)
2. [Sonarqube Installations](#sonarqube-installations)  
3. [Sonarqube-Setup](#sonarqube-setup)
4. [Contact Information](#contact-information)


## Jenkins Installations 
Refer this [link for POC](https://github.com/Cloud-NInja-snaatak/Documentation/tree/himanshu-SCRUM-176/application_ci/tools/setup/sonarqube/software_configuration/poc).

## Sonarqube Installations
Refer this [link for POC](https://github.com/Cloud-NInja-snaatak/Documentation/tree/himanshu-SCRUM-176/application_ci/tools/setup/sonarqube/software_configuration/poc).

## Sonarqube Setup

#### Create Token For a Project on Sonarqube Dashboard 
![Screenshot 2025-05-28 094317](https://github.com/user-attachments/assets/54b4caef-5c70-4439-9c7f-c70cbf125c6a)




#### Select Credential to setup Tokens 
![java_dec4](https://github.com/user-attachments/assets/85315983-27ad-4376-8069-e9f81c83607d)

#### Create Tokens for Sonar 
![java_dec5](https://github.com/user-attachments/assets/b3580319-5e05-4395-9cdd-472084844acb)

####  Install Plugin Sonar-scanner 
![java_dec6](https://github.com/user-attachments/assets/27e8ae35-bfdf-461b-ae5c-81faa4297161)

#### Create Sonarqube Environment 
![java_dec7](https://github.com/user-attachments/assets/5f4714c7-5c18-40cf-9a35-118d31d523b7)

#### Declarative script 
<details>
node {
    def mvnHome = tool name: 'mvn', type: 'maven'
    env.PATH = "${mvnHome}/bin:${env.PATH}"
    env.SONARQUBE_URL = 'http://<your-sonarqube-ip>:9000'
    env.SONAR_PROJECT_KEY = 'scripted'

    stage('Checkout') {
        git branch: 'master', url: 'https://github.com/shubhamprasadnr/secretsanta-generator.git'
    }

    stage('Build') {
        sh 'mvn clean verify'
    }

    stage('SonarQube Scan') {
        withSonarQubeEnv('sonarQube') {
            withCredentials([string(credentialsId: 'javacred', variable: 'SONARQUBE_TOKEN')]) {
                sh """
                mvn sonar:sonar \
                -Dsonar.projectKey=${env.SONAR_PROJECT_KEY} \
                -Dsonar.host.url=${env.SONARQUBE_URL} \
                -Dsonar.login=${SONARQUBE_TOKEN}
                """
            }
        }
    }
}


</details>

#### Run pipeline Successfully 
![Screenshot 2025-05-28 100611](https://github.com/user-attachments/assets/7c620e20-fc71-403d-9d08-8e1ed8489d5f)


#### Result On Sunarqube Dashboard 
![Screenshot 2025-05-28 100839](https://github.com/user-attachments/assets/b1b732ec-a6fb-446d-9a57-37bc2211580a)



##  Contact Information

| Name | Email Address |
|------|---------------|
| Shubham Prasad | [shubham.prasad.snaatak@mygurukulam.co](mailto:shubham.prasad.snaatak@mygurukulam.co) |





