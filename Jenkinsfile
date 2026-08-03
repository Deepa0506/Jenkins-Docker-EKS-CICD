pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "391119427536.dkr.ecr.ap-south-1.amazonaws.com/devops-project"
        IMAGE_NAME = "devops-project"
    }

    stages {

        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME} .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f devops-container || true
                docker run -d --name devops-container -p 8081:80 ${IMAGE_NAME}
                '''
            }
        }

        stage('Login to Amazon ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REPO}
                '''
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh '''
                docker tag ${IMAGE_NAME}:latest ${ECR_REPO}:latest
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker push ${ECR_REPO}:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
