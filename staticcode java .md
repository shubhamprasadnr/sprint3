# POC For Java - Declarative Jenkins Pipeline


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

![java_dec3](https://github.com/user-attachments/assets/cbd0f3a5-f5e2-4ddd-afac-8db15eb6b63d)


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

pipeline {
    agent any
    tools {
        maven 'mvn'  
    }
    environment {
        SONAR_PROJECT_KEY = 'java'
    }
   
    stages {
         stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/shubhamprasadnr/secretsanta-generator.git' // Updated repo URL
            }
        }
        
         stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        // ... other stages ...

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {  // 'sonarqube' must match the server name in Jenkins config
                    withCredentials([string(credentialsId: 'sonarcred', variable: 'SONARQUBE_TOKEN')]) {
                        sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.login=${SONARQUBE_TOKEN}
                        """
                    }
                }
            }
        }
    }
}

      
           

</details>

#### Run pipeline Successfully 
![java_dec8](https://github.com/user-attachments/assets/443f5d95-f86b-46eb-b274-4f663c77d41b)

#### Result On Sunarqube Dashboard 

![java_dec9](https://github.com/user-attachments/assets/f98688f9-8ff7-4cd4-8ef7-33400449d217)

##  Contact Information

| Name | Email Address |
|------|---------------|
| Shubham Prasad | [shubham.prasad.snaatak@mygurukulam.co](mailto:shubham.prasad.snaatak@mygurukulam.co) |




