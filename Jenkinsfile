pipeline {
    agent any

    environment {
        NODE_VERSION = "18"
        IMAGE_NAME = "my-node-app"
        CONTAINER_NAME = "my-node-app-container"
        PORT = "3000"
    }

    stages {
        stage('Install Node.js') {
            steps {
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
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Deploy Docker Container') {
            steps {
                sh '''
                # Stop and remove existing container if it exists
                if [ $(docker ps -a -q -f name=$CONTAINER_NAME) ]; then
                    docker stop $CONTAINER_NAME
                    docker rm $CONTAINER_NAME
                fi

                # Run new container
                docker run -d -p $PORT:$PORT --name $CONTAINER_NAME $IMAGE_NAME
                '''
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
