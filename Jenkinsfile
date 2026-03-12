pipeline {
    agent { label 'linux' }   // replace linux with your exact EC2 node label

    environment {
        IMAGE_NAME = 'sayanth121/jenkins-demo:latest'
    }

    stages {
        stage('Debug Agent') {
            steps {
                sh 'echo "User: $(whoami)"'
                sh 'echo "PWD: $(pwd)"'
                sh 'uname -a'
                sh 'docker version'
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}
