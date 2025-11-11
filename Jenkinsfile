pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker image') {
            steps {
                script {
                    sh 'docker build -t fastapi-lab:latest .'
                }
            }
        }
        stage('Run container (test)') {
            steps {
                script {
                    sh 'docker run -d -p 8000:8000 --name fastapi-test fastapi-lab:latest || true'
                    sleep 5
                    sh 'curl -f http://localhost:8000 || (docker logs fastapi-test && exit 1)'
                    sh 'docker stop fastapi-test && docker rm fastapi-test'
                }
            }
        }
    }
}
