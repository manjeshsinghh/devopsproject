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
