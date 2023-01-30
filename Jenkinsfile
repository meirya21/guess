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

    //Master : get the latest tag number and insert it to values.yaml file
    stage('get tag'){
        env.BRANCH_NAME == 'master'
        sh '''#!/bin/bash
        # Retrieve the latest tag number from Docker Hub
        url="https://hub.docker.com/v2/repositories/meir215/hello_world/tags/"
        TAG=$(curl -s $url | jq -r \'.results[].name\')
        echo "$TAG"

        # Replace the existing tag number in your Helm values file with the latest tag number
        sed -i "s/tag: .*/tag: $TAG/g" guess/values.yaml
        '''}
    
    stage("Commit") {
        env.BRANCH_NAME == 'master'
            sh '''
            git changelog: false, credentialsId: 'git', poll: false, url: 'https://github.com/meirya21/guess.git'
            git add . && git commit -am "[Jenkins CI] guess/valus.yaml"
                '''
        }

    stage("Push") {
        env.BRANCH_NAME == 'master'
            sh '''
            git config --local credential.helper "!f() { echo username=$GIT_AUTH_USR; echo password=$GIT_AUTH_PSW; }; f"
            git push origin master -f 
            '''
    }
}