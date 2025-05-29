pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/kimjaeyou/JenkinsTest.git'
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
