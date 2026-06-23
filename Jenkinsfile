pipeline {
    agent any

    environment {
        // Replace with your actual Docker Hub username
        DOCKER_HUB_USER = 'Ashasarthiwika'
        IMAGE_NAME      = 'periovoice-ai'
        IMAGE_TAG       = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Pulls down the fresh code changes from your GitHub
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building PerioVoice AI Docker Image...'
                sh "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Publishing to Registry...'
                // Inside Jenkins, you will bind your Docker Hub password to 'DOCKER_HUB_PWD'
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_HUB_PWD')]) {
                    sh "echo ${DOCKER_HUB_PWD} | docker login -u ${DOCKER_HUB_USER} --password-stdin"
                    sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete. Cleaning up workspace...'
            sh "docker logout"
        }
    }
}