<<<<<<< HEAD
pipeline {
    agent any

    environment {
        DOCKER_USERNAME = credentials('docker-username')
        DOCKER_PASSWORD = credentials('docker-password')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/manjeshsinghh/devopsproject.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker-compose -f docker-compose.yml build'
            }
        }

        stage('Start Containers') {
            steps {
                sh 'docker-compose -f docker-compose.yml up -d'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'docker-compose -f docker-compose.yml exec -T nlp-app pytest tests/'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    docker-compose -f docker-compose.yml push
                '''
            }
        }

        stage('Stop Containers') {
            steps {
                sh 'docker-compose -f docker-compose.yml down'
            }
        }
    }

    post {
        always {
            sh 'docker system prune -f'
        }
    }
}
=======
pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository containing your Dockerfile and docker-compose.yml
                git 'https://github.com/manjeshsinghh/devopsproject.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker images using Docker Compose
                    sh 'docker-compose -f docker-compose.yml build'
                }
            }
        }

        stage('Start Containers') {
            steps {
                script {
                    // Start the containers in detached mode
                    sh 'docker-compose -f docker-compose.yml up -d'
                }
            }
        }

        stage('Run Tests (Optional)') {
            steps {
                script {
                    // If you have tests or checks, run them here (e.g., container health check)
                    sh 'docker-compose -f docker-compose.yml exec nlp-app pytest tests/'  // Modify as needed
                }
            }
        }

        stage('Push Docker Image to Registry') {
            steps {
                script {
                    // Log in to Docker registry (Docker Hub in this case)
                    docker.withRegistry('https://docker.io', 'docker-hub-credentials') {
                        // Push the Docker images to Docker Hub or your registry
                        sh 'docker-compose -f docker-compose.yml push'
                    }
                }
            }
        }

        stage('Stop Containers') {
            steps {
                script {
                    // Shut down the containers after the work is done
                    sh 'docker-compose -f docker-compose.yml down'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker resources after pipeline execution
            sh 'docker system prune -f'
        }
    }
}

>>>>>>> 9031ca952db98c0c01c065bbedfee77cd0a2ea24
