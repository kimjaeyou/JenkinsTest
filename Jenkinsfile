pipeline {
    agent any

    environment {
        IMAGE_NAME = 'kimjaeyoung98/my-node-app-jenkins'
        VERSION = "build-${env.BUILD_ID}"
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/kimjaeyou/JenkinsTest.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${VERSION}")
                }
            }
        }
        stage('DockerHub Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub1', passwordVariable: 'DOCKER_PW', usernameVariable: 'DOCKER_USER')]) {
                    sh '''
                        echo "$DOCKER_PW" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag ${IMAGE_NAME}:${VERSION} ${IMAGE_NAME}:latest
                        docker push ${IMAGE_NAME}:${VERSION}
                        docker push ${IMAGE_NAME}:latest
                    '''
                }
            }
        }

        stage('Deploy to Windows Server') {
            steps {
                sshagent(['deploy-key']) {
                sh '''
    ssh -o StrictHostKeyChecking=no kosta@192.168.0.3 whoami
    ssh -o StrictHostKeyChecking=no kosta@192.168.0.3 dir "C:\\Users\\KOSTA\\deploy"
'''

                }
            }
        }


    }
}
