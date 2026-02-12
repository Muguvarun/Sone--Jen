pipeline {
    agent any
    environment {
        IMAGE_NAME = "myapp"
        IMAGE_TAG = "1.0"
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Validate Files') {
            steps {
                sh 'test -f index.html || exit 1'
                sh 'test -f Dockerfile || exit 1'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker rm -f myapp || true'
                sh 'docker run -d -p 8090:80 --name myapp $IMAGE_NAME:$IMAGE_TAG'
            }
        }
        stage('Smoke Test') {
            steps {
                sh 'curl http://localhost:8090'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
