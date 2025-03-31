pipeline {
    agent any

    environment {
        IMAGE_NAME = "test-php"
        DOCKER_REGISTRY = "docker.io"  // e.g., dockerhub, or your private registry
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull the latest changes from the repo
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t $DOCKER_REGISTRY/$IMAGE_NAME .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker registry and push the image
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD $DOCKER_REGISTRY'
                    sh 'docker push $DOCKER_REGISTRY/$IMAGE_NAME'
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                script {
                    // Assuming you deploy by pulling the image on your server
                    sh '''
                        ssh user@server "docker pull $DOCKER_REGISTRY/$IMAGE_NAME"
                        ssh user@server "docker stop php-app || true"
                        ssh user@server "docker rm php-app || true"
                        ssh user@server "docker run -d --name php-app -p 8080:80 $DOCKER_REGISTRY/$IMAGE_NAME"
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
