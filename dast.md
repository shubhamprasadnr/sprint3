pipeline {
    agent any
    
     environment {
        PYTHON = "/usr/bin/python3" // Explicit Python path
        VENV_DIR = "venv"
    }

    stages {
        stage('git_checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/OT-MICROSERVICES/attendance-api.git'
            }
        }
        
        stage('virtual_Env') {
            steps {
                sh '''
                 ${PYTHON} -m venv ${VENV_DIR}
                 ${VENV_DIR}/bin/pip install --upgrade pip
                 ${VENV_DIR}/bin/pip install pipreqs
                 ${VENV_DIR}/bin/pipreqs . --force
                 '''
            }
        }
        
        stage('DAST with ZAP CLI') {
            steps {
                sh """
                    zaproxy -cmd \
                    -port 8090 \
                    -quickurl http://54.237.233.167:8080/apidocs/ \
                    -quickout ${WORKSPACE}/zap-report.html \
                    -quickprogress
                    """
            }
        }
    //     stage('Generate report'){
    //         steps{
                
    //       publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: '.', reportFiles: 'zap-report.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])




    //         }
    //     } 
   }
}
