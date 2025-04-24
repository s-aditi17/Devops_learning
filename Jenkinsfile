node {
    def nodeVersion = 'v14.19.0'
    def npmVersion = '8.5.2'
    def angularCliVersion = '12.2.16'

    stage('Install Node.js') {
        echo "Installing Node.js ${nodeVersion}..."
        sh '''
            # Remove any previous Node.js versions
            sudo rm -rf /usr/local/bin/node /usr/local/bin/npm /usr/local/lib/node_modules

            # Download and install specific Node.js version
            curl -O https://nodejs.org/dist/v14.19.0/node-v14.19.0-linux-x64.tar.xz
            tar -xf node-v14.19.0-linux-x64.tar.xz
            sudo cp -r node-v14.19.0-linux-x64/* /usr/local/

            # Verify versions
            node -v
            npm -v
        '''
    }

    stage('Install npm version') {
        echo "Installing npm ${npmVersion}..."
        sh "npm install -g npm@${npmVersion}"
        sh "npm -v"
    }

    stage('Install Angular CLI') {
        echo "Installing Angular CLI ${angularCliVersion}..."
        sh "npm install -g @angular/cli@${angularCliVersion}"
        sh "ng version"
    }

    stage('Clone Repository') {
        git credentialsId: 'github-credentials', url: 'https://github.com/s-aditi17/Devops_learning.git'
    }

    stage('Install Dependencies') {
        sh 'npm install'
    }

    stage('Build Angular App') {
        sh 'ng build --configuration production'
    }

    stage('Archive Artifacts') {
        archiveArtifacts artifacts: 'dist/**/*', allowEmptyArchive: true
    }

    stage('Clean Workspace') {
        cleanWs()
    }
}
