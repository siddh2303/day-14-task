pipeline {
    agent any

    environment {
        //DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
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
                    //def dockerImage = docker.build("${env.IMAGE_NAME}")
                    sh "docker buildx build -t ${env.IMAGE_NAME} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "dockerhub-credentials-id") {
                        //dockerImage.push('latest')
                        sh "docker push ${env.IMAGE_NAME}:latest"
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
