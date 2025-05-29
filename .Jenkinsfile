pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/사용자/my-node-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-node-app')
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name my-node-test my-node-app || true'
            }
        }
    }
}
