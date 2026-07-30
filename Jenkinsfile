pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-project .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f devops-container || true
                docker run -d --name devops-container -p 8081:80 devops-project
                '''
            }
        }
    }
}
