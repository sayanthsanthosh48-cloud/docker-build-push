pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "sayanth121/jenkins-demo"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME%:latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                bat "echo %DOCKERHUB_CREDENTIALS_PSW% | docker login -u %DOCKERHUB_CREDENTIALS_USR% --password-stdin"
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                bat "docker push %IMAGE_NAME%:latest"
            }
        }
    }

    post {
        always {
            bat "docker logout"
        }
    }
}
