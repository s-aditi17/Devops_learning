pipeline {
    agent any

    tools {
        nodejs 'NodeJS 14.19.0'  
    }

    environment {
        NPM_VERSION = '8.5.2'
        ANGULAR_CLI_VERSION = '12.2.16'
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

        stage ('Sonarqube scan'){
           steps{
            script {
              scannerHome = tool 'sonar'
                }
                withSonarQubeEnv('sonar') {
              sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=my-angular-app -Dsonar.sources=src"
                  }
                }
              }
        stage('Download SonarQube Report') {
    steps {
        script {
            def sonarHost = 'http://18.215.144.54:9000'
            def projectKey = 'my-angular-app'
            def authToken = 'squ_94c360b1e0e4bf5e9396a0091dca5d0f2dcd276f'
 
            // Download Issues Report (JSON format)
            sh """
                curl -u ${authToken}: \
                '${sonarHost}/api/issues/search?componentKeys=${projectKey}&resolved=false' \
                -o sonar-report.json
            """
 
            // Archive the JSON report (optional)
            archiveArtifacts artifacts: 'sonar-report.json', fingerprint: true
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
