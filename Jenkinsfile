pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-flask-app"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sazaan/Flask-CI-CD-Pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Remove Existing Container') {
            steps {
                script {
                    // Stop and remove the existing container if it exists
                    sh '''
                    docker ps -a | grep flask-app && docker stop flask-app && docker rm flask-app || true
                    '''
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    sh "docker run -d -p 5000:5000 --name flask-app ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }

        stage('Clean Up Old Images') {
            steps {
                script {
                    // Remove unused images to save space
                    sh "docker image prune -f"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed. Please check the logs.'
        }
    }
}
// this is a test for webhooks
