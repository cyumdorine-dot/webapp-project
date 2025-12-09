pipeline {
    agent any

    environment {
        NODE_VERSION = "18"
    }

    stages {
        stage('Install Node.js') {
            steps {
                // Install Node.js if not already installed
                sh '''
                if ! command -v node > /dev/null; then
                    curl -fsSL https://deb.nodesource.com/setup_$NODE_VERSION.x | sudo -E bash -
                    sudo apt-get install -y nodejs
                fi
                node -v
                npm -v
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests defined in package.json
                sh 'npm test'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
