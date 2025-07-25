pipeline {
    agent any

    environment {
        COMPOSE_DIR = 'setup-deploy-docker-compose'
        COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/vivekdongare81/fintech-event-driven-microservices.git'
            }
        }

        stage('Start Docker Services') {
            steps {
                dir("${COMPOSE_DIR}") {
                    script {
                        sh 'docker-compose down' // stop existing containers if any
                        sh 'docker-compose pull' // pull base images if needed
                        sh 'docker-compose up -d --build' // build and start containers
                    }
                }
            }
        }

        stage('Wait for Services to Stabilize') {
            steps {
                echo 'Waiting for services to be ready...'
                sleep time: 20, unit: 'SECONDS' // optional, or use healthchecks
            }
        }

        stage('Show Running Containers') {
            steps {
                sh 'docker ps'
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed!'
        }
        success {
            echo 'Services started successfully!'
        }
    }
}
