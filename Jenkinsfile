pipeline {
    agent any
    environment {
        IMAGE_NAME = "myapp"
        IMAGE_TAG = "1.0"
    }
    stages {
        stage('Checkout Code') {
                steps { 
                 git branch: 'main', 
                    url: 'https://github.com/Muguvarun/Sone--Jen.git', 
                    credentialsId: 'github-token'
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
                sh 'docker build -t sonejen-app:latest .'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker rm -f myapp || true'
                sh 'docker run -d -p 8090:80 --name myapp :latest npm test || echo "No tests defined"'
            }
        }
        stage('Deploy') { steps { sh ''' docker stop sonejen-app || true 
        docker rm sonejen-app || true 
        docker run -d --name sonejen-app -p 8080:80 sonejen-app:latest 
        ''' 
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
