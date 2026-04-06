pipeline {
   agent any
   environment {
       // ID must match the credentials ID set in Jenkins
	   DOCKER_USER = "vnddocker501"   //docker user name
       DOCKER_CRED_ID = 'dockerhub-cred'    //id we created in credentials of docker as token
       // Replace with your Docker Hub username and image name
       IMAGE_NAME = 'vnddocker501/pipeline-itrack' 
       IMAGE_TAG = "latest"
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
     stage('Docker Login and Operations') {
          steps {
             withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', passwordVariable: 'DOCKER_CRED_ID', usernameVariable: 'DOCKER_USER')]) {
                 sh "docker login -u ${env.DOCKER_USER} -p ${env.DOCKER_CRED_ID}"
                 // Build command (tag with your username for push access)
                 sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                 // Push command (uses the active login session)
                 sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
                 sh "docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:${IMAGE_TAG}"
                 sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                 // Pull command (uses the active login session)
			     //sh "docker pull ${IMAGE_NAME}:${BUILD_NUMBER}"
                 sh "docker pull ${IMAGE_NAME}:${IMAGE_TAG}"
                 sh "docker logout" // Optional but good practice
                }
            }
        }
     stage('Deploy to cluster') {
       steps {
         sh 'gcloud config set project vinodlearning-489104'
		 sh 'gcloud container clusters get-credentials itrack-dev-vnd --zone us-central1-a --project vinodlearning-489104'
       }
     }
   }
}
