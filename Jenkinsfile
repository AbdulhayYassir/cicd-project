pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-reg'
        IMAGE_NAME = '3booda24/jenkins-nodejs:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'Master', url: 'https://github.com/abdelrahmanonline4/GitOps-ci-cd-with-Jenkins-and-Argocd'
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        echo 'Logged in to Docker Hub'
                    }
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    def app = docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        def app = docker.image("${IMAGE_NAME}")
                        app.push('latest')
                    }
                }
            }
        }
    }
}
