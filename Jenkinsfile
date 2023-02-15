node {
    // Define all variables
    def appName = 'num_guess' 
    def imageTag = "meir215/${appName}"
    def buildnum = "1.0.${env.BUILD_NUMBER}"
    def dockerImage

    // Checkout Code from Git
    checkout scm

    // Build the docker image
    stage('Build image') {
        if (env.BRANCH_NAME == 'master') {
            dockerImage = docker.build("${imageTag}:${buildnum}")
        }
    }

    // E2E testing
    stage('E2E testing') {
        if (env.BRANCH_NAME == 'master') {
            sh 'echo test'
        }
    }

    // Push the image to dockerhub
    stage('Push image to registry') {
        if (env.BRANCH_NAME == 'master') {
            withDockerRegistry([ credentialsId: "docker", url: "https://hub.docker.com/repository/docker/meir215/guess" ]) {
                dockerImage.push()
            }
        }
    }
}
