pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t flask-app:latest .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm -d -p 5000:5000 --name test-app flask-app:latest'
                sh 'sleep 5'
                sh 'curl -f http://localhost:5000'
                sh 'docker stop test-app'
            }
        }
    }
}

