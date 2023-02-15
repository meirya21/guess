pipeline {
    agent any

    environment {
        APP_NAME = 'num_guess'
        IMAGE_TAG = "meir215/${APP_NAME}"
        BUILD_VERSION = "1.0.${BUILD_NUMBER}"
        DOCKER_REGISTRY_URL = 'https://hub.docker.com/repository/docker/meir215/guess'
    }

    stages {
        stage('Install Docker') {
            steps {
                sh 'curl -fsSL https://get.docker.com -o get-docker.sh'
                sh 'sh get-docker.sh'
            }
        }

        stage('Build Docker image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    def dockerImage = docker.build("${IMAGE_TAG}:${BUILD_VERSION}")
                    dockerImage.push()
                }
            }
        }

        stage('E2E testing') {
            when {
                branch 'master'
            }
            steps {
                sh 'echo test'
            }
        }

        stage('Push Docker image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    withDockerRegistry([ credentialsId: "docker", url: DOCKER_REGISTRY_URL ]) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
