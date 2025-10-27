pipeline {
    agent any

    environment {
        IMAGE_NAME = "nodejs-app"
        CONTAINER_NAME = "nodejs-container"
        PORT = "3000"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Pulling source code from GitHub..."
                git branch: 'nodejs-docker-task', url: 'https://github.com/AmroFouda/efe-senior-tasks-2025.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Run Container') {
            steps {
                echo "Running Docker container..."
                script {
                    // Stop and remove any old container
                    sh """
                        docker ps -q --filter name=${CONTAINER_NAME} | grep -q . && docker stop ${CONTAINER_NAME} && docker rm ${CONTAINER_NAME} || true
                        docker run -d --name ${CONTAINER_NAME} -p ${PORT}:${PORT} ${IMAGE_NAME}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Build and deployment completed successfully!"
            sh "docker ps | grep ${CONTAINER_NAME}"
        }
        failure {
            echo " Build or deployment failed!"
        }
    }
}
