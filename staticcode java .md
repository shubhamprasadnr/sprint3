





## Setup Sonarqube 

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
<summary>
pipeline {
    agent any
    
    
    tools{
        maven 'mvn'  
    }

    environment {
        SONARQUBE_URL = 'http://16.16.187.233:9000/' // Update with your SonarQube server URL
        SONAR_PROJECT_KEY = 'java' // Update with your actual SonarQube project key
    }

    stages {
        stage('Cleanup Workspace') {
          steps {
        cleanWs()
           }
         }
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

        stage('sonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') { // Ensure 'demo' matches the SonarQube instance name in Jenkins settings
                    withCredentials([string(credentialsId: 'sonarcred', variable: 'SONARQUBE_TOKEN')]) {
                        sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.host.url=${SONARQUBE_URL} \
                        -Dsonar.login=${SONARQUBE_TOKEN}
                        """
                    }
                }
            }
        }
    }
}

</summary>

#### Run pipeline Successfully 
![java_dec8](https://github.com/user-attachments/assets/443f5d95-f86b-46eb-b274-4f663c77d41b)



