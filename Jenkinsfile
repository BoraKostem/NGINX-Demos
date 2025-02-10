pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = 'borakostem/nginx-hello'
        DOCKER_CREDENTIALS = 'DOCKER_HUB_CREDENTIALS'
        GITHUB_CREDENTIALS = 'GITHUB_CREDENTIALS'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: env.GITHUB_CREDENTIALS, url: 'https://github.com/BoraKostem/NGINX-Demos.git', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dir('nginx-hello') {
                        sh 'docker build -t $DOCKER_HUB_REPO:latest .'
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: env.DOCKER_CREDENTIALS, url: '']) {
                        sh 'docker push $DOCKER_HUB_REPO:latest'
                    }
                }
            }
        }
    }

    post {
        success {
            build job: 'Analyze_Docker_Image', wait: false
        }
    }
}
