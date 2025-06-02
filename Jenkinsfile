pipeline {
    agent any

    environment {
        IMAGE_NAME = 'kimjaeyoung98/my-node-app-jenkins'
        VERSION = "build-${env.BUILD_ID}"
    }

    options {
        skipDefaultCheckout() // 'Clone' 스테이지에서 직접 Git 수행
        timestamps() // 콘솔 출력에 시간 표시
    }

    triggers {
        githubPush() // GitHub Webhook 기반 자동 실행 트리거
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/kimjaeyou/JenkinsTest.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${VERSION}", "--no-cache .")
                }
            }
        }

        stage('Push to DockerHub') {
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
                        ssh -o StrictHostKeyChecking=no kosta@192.168.0.3 powershell -ExecutionPolicy Bypass -File "C:\\Users\\KOSTA\\deploy\\deploy.ps1" ${VERSION}
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ 배포 성공: ${VERSION}"
        }
        failure {
            echo "❌ 배포 실패. Jenkins 콘솔에서 로그를 확인하세요."
        }
    }
}
