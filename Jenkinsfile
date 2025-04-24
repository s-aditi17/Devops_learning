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

        stage('Deploy to S3') {
            steps {
                sh '''
                    export PATH=$PWD/node-${NODE_VERSION}-linux-x64/bin:$PATH

                    echo "Deploying to S3..."

                    # Ensure unzip is available before it's used
                    if ! command -v unzip &> /dev/null; then
                        echo "Installing unzip..."
                        apt-get update && apt-get install -y unzip
                    fi

                    # Install AWS CLI if not already installed
                    if ! command -v aws &> /dev/null; then
                        echo "Installing AWS CLI..."
                        curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                        unzip awscliv2.zip
                        ./aws/install -i $PWD/aws-cli -b $PWD/aws-cli-bin
                        export PATH=$PWD/aws-cli-bin:$PATH
                    fi

                    # Sync build output to S3
                    aws s3 sync dist/ s3://jenkins-angular-bucket/
                '''
            }
        }
    }

}

