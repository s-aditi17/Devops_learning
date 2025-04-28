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
      stage('SonarQube Analysis') {    
            environment {
                SONAR_SCANNER_HOME = tool 'sonar'
            }
            steps {
                withSonarQubeEnv('sonar') { 
                    sh '''
                        ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=my-angular-project \
                        -Dsonar.projectName="My Angular App" \
                        -Dsonar.sources=src \
                        -Dsonar.exclusions=**/*.spec.ts \
                        -Dsonar.language=ts \
                        -Dsonar.typescript.tsconfigPath=tsconfig.json
                    '''
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
        withEnv(["PATH=/usr/local/bin:$PATH"]) { // ADD THIS
            sh '''
                echo Deploying to S3...
                aws s3 sync dist/kickstart-angular s3://jenkins-angular-bucket/
            '''
        }
    }
}
    }
}
