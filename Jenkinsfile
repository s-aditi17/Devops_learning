pipeline {
    agent any

    tools {
        nodejs 'NodeJS 14.19.0'  
    }

    environment {
        NPM_VERSION = '8.5.2'
        ANGULAR_CLI_VERSION = '12.2.16'
        SONARQUBE_SERVER = 'sonar'  
        SONARQUBE_TOKEN = credentials('sonar')
    }

    stages {
        stage('Install npm and Angular CLI') {
            steps {
                sh '''
                    npm install -g npm@${NPM_VERSION}
                    npm install -g @angular/cli@${ANGULAR_CLI_VERSION}
                    ng version
                '''
            }
        }

        stage('Clone Repo') {
            steps {
                git 'https://github.com/s-aditi17/Devops_learning.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis using sonar-scanner 
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh '''
                            sonar-scanner \
                                -Dsonar.projectKey=Devops_learning \
                          
                               
                        '''
                    }
                }
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
