pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-flask-app'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh "docker run --rm -d -p 5000:5000 --name flask-test ${DOCKER_IMAGE}:${DOCKER_TAG}"
                sleep 10  // Wait for the app to start
                sh "curl http://localhost:5000"
                sh "docker stop flask-test"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the Docker container...'
                sh "docker run -d -p 5000:5000 --name flask-app ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

