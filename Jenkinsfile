pipeline {
    agent any

    environment {
        NODE_VERSION = 'v14.19.0'
        NPM_VERSION = '8.5.2'
        ANGULAR_CLI_VERSION = '12.2.16'
    }

    stages {
        stage('Download Node.js') {
            steps {
                sh '''
                    echo "Downloading Node.js ${NODE_VERSION}..."
                    curl -O https://nodejs.org/dist/${NODE_VERSION}/node-${NODE_VERSION}-linux-x64.tar.xz
                    tar -xf node-${NODE_VERSION}-linux-x64.tar.xz
                    export PATH=$PWD/node-${NODE_VERSION}-linux-x64/bin:$PATH
                    node -v
                    npm -v
                '''
            }
        }

        stage('Install npm and Angular CLI') {
            steps {
                sh '''
                    export PATH=$PWD/node-${NODE_VERSION}-linux-x64/bin:$PATH
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
                sh '''
                    export PATH=$PWD/node-${NODE_VERSION}-linux-x64/bin:$PATH
                    npm install
                '''
            }
        }

        stage('Build Angular App') {
            steps {
                sh '''
                    export PATH=$PWD/node-${NODE_VERSION}-linux-x64/bin:$PATH
                    ng build --configuration production
                '''
            }
        }

    }
}

