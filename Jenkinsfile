pipeline {
    agent any

    tools {
        nodejs 'NodeJS 14.19.0'  
    }

    environment {
        NPM_VERSION = '8.5.2'
        ANGULAR_CLI_VERSION = '12.2.16'
        SONARQUBE_SERVER = 'sonar'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh '''
                        echo "Running SonarQube Scanner..."
                        sonar-scanner \
                          -Dsonar.projectKey=kickstart-app \
                          -Dsonar.sources=src 
                         
                    '''
                }
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'ng build --configuration production'
            }
        }
    }
}
