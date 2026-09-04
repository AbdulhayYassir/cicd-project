pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'Dockerhub-cred'
        IMAGE_REPO = "3booda24/nodejs8"
        IMAGE_TAG  = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'Master',
                    url: 'https://github.com/abdelrahmanonline4/GitOps-ci-cd-with-Jenkins-and-Argocd'
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    echo 'Logging in to Docker Hub...'

                    docker.withRegistry(
                        'https://index.docker.io/v1/',
                        "${DOCKER_CREDENTIALS_ID}"
                    ) {
                        echo 'Login successful'
                    }
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    echo 'Building Docker image...'

                    docker.build("${IMAGE_REPO}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    echo 'Pushing image to Docker Hub...'

                    docker.withRegistry(
                        'https://index.docker.io/v1/',
                        "${DOCKER_CREDENTIALS_ID}"
                    ) {
                        def app = docker.image("${IMAGE_REPO}:${IMAGE_TAG}")
                        app.push()
                    }
                }
            }
        }

        stage('CD - Deploy to Kubernetes') {
            steps {
                sh '''
                    cd kubernetes
                    kubectl apply -f .
                '''
            }
        }
    }
}
