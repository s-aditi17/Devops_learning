pipeline {
    agent any

    environment {
       NODE_HOME = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
       PATH = "${NODE_HOME}/bin:${env.PATH}"
            
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
            script {
            scannerHome = tool 'sonar'
            }
                withSonarQubeEnv("sonar") {                        
                       sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Devops_learning -Dsonar.sources=src"
                }
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build'
            }
        }
    }
}
