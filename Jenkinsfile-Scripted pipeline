pipeline {
   agent any
    environment {
       // ID must match the credentials ID set in Jenkins
       DOCKER_CRED_ID = 'dockerhub-cred' 
       // Replace with your Docker Hub username and image name
       IMAGE_NAME = 'vnddocker501/pipeline-itrack' 
    }
   stages {
     stage ("checkout") {
       steps {
         checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-cred', url: 'https://github.com/sonyaws501/addressbook.git']])
       }
     }
     stage('Build') {
       steps {
		 sh 'whoami'
         sh '/opt/maven/bin/mvn package'
       }
     }
     stage('Build Docker Image') {
       steps {
          script {
            // Use withRegistry to authenticate and push the image
		    dockerImage = docker.build "${IMAGE_NAME}:${env.BUILD_NUMBER}"
            //sh "docker build -t ${IMAGE_NAME}.${BUILD_NUMBER} ."
            docker.withRegistry( '', DOCKER_CRED_ID ) {
            dockerImage.push()
            dockerImage.push('latest') // Optional: push with 'latest' tag as well
            }
           }
        }
     }
     stage('Remove Unused docker image') {
       steps{
         sh "docker rmi $IMAGE_NAME:$BUILD_NUMBER"
         sh "docker rmi $IMAGE_NAME:latest"
      }
    }
   }
}
