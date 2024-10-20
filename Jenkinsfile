pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Install and use Node.js v18.20.3 via nvm
                sh '''
                    # Install nvm if not already installed
                    if [ ! -s "$NVM_DIR/nvm.sh" ]; then
                        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
                        . ~/.nvm/nvm.sh
                    fi

                    # Load nvm and use the correct Node.js version
                    . ~/.nvm/nvm.sh
                    nvm install 18.20.3
                    nvm use 18.20.3

                    # Verify the node version
                    node --version
                '''

                // Run the build commands
                sh 'ls'
                sh 'npm install'
                sh 'echo N | ng analytics off'
                sh 'ng build'
                sh 'ls'
                sh 'cd dist && ls'
                sh 'cd dist/angular-tour-of-heroes/browser && ls'
            }
        }
        stage('S3 Upload') {
            steps {
                withAWS(region: 'us-east-1', credentials: 'd03ccb78-9671-44c1-ac0a-8a5d7a5db9a8') {
                    sh 'ls -la'
                    sh 'aws s3 cp dist/angular-tour-of-heroes/browser/. s3://concal-jenkins-angular/ --recursive'
                }
            }
        }
    }
}
