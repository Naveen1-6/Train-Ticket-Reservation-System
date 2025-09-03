pipeline {
    agent any

    environment {
        IMAGE_NAME = "train-ticket-reservation-system"
        IMAGE_TAG  = "latest"
        CONTAINER_NAME = "ttrs-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Naveen1-6/Train-Ticket-Reservation-System.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        echo "Building Docker image..."
                        docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                        docker build -t ${IMAGE_NAME}:${IMAGE_TAG} -f docker/Dockerfile .

                    """
                }
            }
        }

        stage('Remove Old Container (if exists)') {
            steps {
                script {
                    sh """
                        if [ \$(docker ps -aq -f name=${CONTAINER_NAME}) ]; then
                            echo "Stopping and removing old container..."
                            docker stop ${CONTAINER_NAME} || true
                            docker rm ${CONTAINER_NAME} || true
                        fi
                    """
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh """
                        echo "Running new container..."
                        docker run -d --name ${CONTAINER_NAME} -p 8080:8080 ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
    }
}

