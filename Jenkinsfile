pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-php-app'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from Git using the Git credentials
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],  // Replace 'main' with your default branch
                        extensions: [],
                        userRemoteConfigs: [
                            [
                                url: 'https://github.com/your-username/your-repo.git',
                                credentialsId: 'github-credentials'  // This is the ID of the Git credentials
                            ]
                        ]
                    ])
                }
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
                script {
                    // Run tests inside the container (for example, checking PHP version)
                    sh 'docker run --rm ${DOCKER_IMAGE}:${DOCKER_TAG} php -v'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the Docker container to your server (example: using Docker Compose)
                    echo "Deploying to production..."
                    sh 'docker run -d -p 9000:9000 ${DOCKER_IMAGE}:${DOCKER_TAG}'
                }
            }
        }
    }
}
