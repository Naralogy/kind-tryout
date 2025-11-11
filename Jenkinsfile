pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build Docker image') {
      steps {
        sh 'docker build -t fastapi-lab:latest .'
      }
    }

    stage('Smoke Test (internal network)') {
      steps {
        sh '''
          set -e
          docker rm -f fastapi-test 2>/dev/null || true
          docker run -d --name fastapi-test fastapi-lab:latest
          echo "Waiting for app to start..."
          sleep 3
          docker run --rm --network container:fastapi-test curlimages/curl:8.10.1 -sf http://127.0.0.1:8000
          docker rm -f fastapi-test
        '''
      }
    }
  }
}
