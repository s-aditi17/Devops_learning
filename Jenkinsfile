
pipeline {
    agent any
    environment {
        NODE_VERSION = '14.19.0'
        NPM_VERSION = '8.5.2'
        ANGULAR_CLI_VERSION = '12.2.16'
        AWS_REGION = 'us-east-1'
        SONARQUBE_PROJECT_NAME = 'kickstar-app'
        SONARQUBE_PROJECT_KEY='kickstart-app'

    }
    tools {
        nodejs "NodeJS"
    }
    stages {
     
        stage ('install dependency') {
            steps {
             script {
                sh "npm install -g npm@${NPM_VERSION}"
                sh "npm install -g @angular/cli@${ANGULAR_CLI_VERSION}"
                sh "npm install"
            }
        }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar' 
                    withSonarQubeEnv(credentialsId: 'sonar')  {  
                    
                        sh "${scannerHome}/bin/sonar-scanner \
-Dsonar.projectKey=${SONARQUBE_PROJECT_KEY} \
-Dsonar.projectName=${SONARQUBE_PROJECT_NAME} \
                            -Dsonar.sources=src \
-Dsonar.javascript.lcov.reportPaths=coverage/lcov-report/lcov-report.json"
                    }
                }
            }
        }
        stage ('build') {
            steps {
                sh 'npm run build'
            }
        }

    }
}

