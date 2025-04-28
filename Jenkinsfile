pipeline {
    agent any

    tools {
        nodejs 'NodeJS 14.19.0'  
    }

    environment {
        NPM_VERSION = '8.5.2'
        ANGULAR_CLI_VERSION = '12.2.16'
        SONARQUBE_SERVER = 'sonar'  // Ensure this matches your configured SonarQube server in Jenkins
        SONARQUBE_TOKEN = credentials('sonar')  // Credentials ID for your SonarQube token
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/s-aditi17/Devops_learning.git'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis using sonar-scanner 
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh '''
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=Devops_learning \
                            -Dsonar.sources=src \
                            -Dsonar.login=${SONARQUBE_TOKEN}
                        '''
                    }
                }
            }
        }
        
        stage('Install npm and Angular CLI') {
            steps {
                sh '''
                    npm install -g npm@${NPM_VERSION}
                    npm install -g @angular/cli@${ANGULAR_CLI_VERSION}
                    ng version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'ng build --configuration production'
            }
        }

        stage('Deploy to S3') {
            steps {
                sh '''
                    echo "Deploying to S3..."
                    aws s3 sync dist/kickstart-angular s3://jenkins-angular-bucket/
                '''
            }
        }
    }
}
