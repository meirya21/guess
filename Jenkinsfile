node {

    //Define all variables
    def appName = 'num_guess' 
    def imageTag = "meir215/${appName}"
    def buildnum = "1.0.${env.BUILD_NUMBER}"

    //Checkout Code from Git
    checkout scm
  
    //master : Build the docker image.
    stage('Build image') {
        env.BRANCH_NAME == 'master'
        sh("docker build . -t ${imageTag}:latest")
        }
    
    //master : E2E testing
    stage('E2E testing') {
        env.BRANCH_NAME == 'master'
         sh 'echo test'
        }

    //master : Push the image to dockerhub
    stage('Push image to registry') {
        env.BRANCH_NAME == 'master'
        withDockerRegistry([ credentialsId: "docker", url: "" ]) {
        sh("docker push ${imageTag}:latest")
        }
    }
}
