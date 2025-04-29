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
        stage('Setup Tools') {
            steps {
                sh '''
                    echo "Installing specific npm and Angular CLI versions..."
                    npm install -g npm@${NPM_VERSION}
                    npm install -g @angular/cli@${ANGULAR_CLI_VERSION}
                    echo "Installed Versions:"
                    node -v
                    npm -v
                    ng version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    echo "Installing project dependencies..."
                    npm install
                '''
            }
        }

        stage('Build Angular App') {
            steps {
                sh '''
                    echo "Building Angular Application..."
                    ng build --configuration production
                '''
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
    }
}
