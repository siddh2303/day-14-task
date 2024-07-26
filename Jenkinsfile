pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
        REPO_URL = 'https://github.com/siddh2303/day-14-task.git'
        IMAGE_NAME = 'siddhpatel/java-app-build'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${env.REPO_URL}", branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${env.IMAGE_NAME}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${env.DOCKERHUB_CREDENTIALS}") {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh 'docker run -d --name java-app-build -p 8090:8090 ${env.IMAGE_NAME}:latest'
                }
            }
        }
    }

    post {
        always {
            //cleanWs()
            echo 'Finished'
        }
    }
}
