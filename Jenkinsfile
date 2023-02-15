//Define all variables
def appName = 'num_guess' 
def imageTag = "meir215/${appName}"
def buildnum = "1.0.${env.BUILD_NUMBER}"

pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock    
        '''
    }
  }
  stages {

    //Checkout Code from Git
    checkout scm
  
    //master : Build the docker image.
    stage('Build image') {
        env.BRANCH_NAME == 'master'
        steps {
            container('docker') {
                dockerImage = docker.build("${imageTag}:${buildnum}")
            }
        }
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
        dockerImage.push()
        }
    }
  }
}
