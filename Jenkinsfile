pipeline {
    agent any
    environment {
        NODE_VERSION = '14.19.0'
        NPM_VERSION = '8.5.2'
        ANGULAR_CLI_VERSION = '12.2.16'
        AWS_REGION = 'us-east-1'
        SONARQUBE_PROJECT_NAME = 'kickstart-app'
        SONARQUBE_PROJECT_KEY = 'kickstart-app'
        NODE_OPTIONS = '--max-old-space-size=4096'
    }
    tools {
        nodejs 'NodeJS'  // Ensure this is configured in your Jenkins Global Tool Configuration
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Install the required versions of npm and Angular CLI globally
                    sh "npm install -g npm@${NPM_VERSION}"
                    sh "npm install -g @angular/cli@${ANGULAR_CLI_VERSION}"

                    // Install local project dependencies
                    sh "npm install"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Ensure that 'sonar' tool is properly configured in Jenkins
                    def scannerHome = tool 'sonar'

                    // Run SonarQube analysis
                    withSonarQubeEnv('sonar') {  // 'sonar' should be the name of your SonarQube server in Jenkins
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=${SONARQUBE_PROJECT_KEY} \
                            -Dsonar.projectName=${SONARQUBE_PROJECT_NAME} \
                            -Dsonar.sources=src \
                            -Dsonar.javascript.lcov.reportPaths=coverage/lcov-report/lcov-report.json"
                    }
                }
            }
        }

        stage('Build') {
            steps {
                // Build Angular app
                sh 'npm run build'
            }
        }
    }
}
