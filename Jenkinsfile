pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'test-php-website01'
        DOCKER_TAG = 'latest'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Test') {
            steps {
                // Here you can add any tests you want to run, for example:
                script {
                    // Run tests (e.g., PHPUnit) inside the container
                    sh 'docker run --rm ${DOCKER_IMAGE}:${DOCKER_TAG} php -v'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the Docker container to your server (example: using Docker Compose)
                    // Or, push the image to a container registry
                    echo "Deploying to production..."
                    sh 'docker run -d -p 9000:9000 ${DOCKER_IMAGE}:${DOCKER_TAG}'
                }
            }
        }
    }
}
