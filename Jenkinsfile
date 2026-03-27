pipeline {
   agent any
   environment {
        //IMAGE_NAME = 'your-dockerhub-user/your-app'
        //DOCKER_IMAGE_NAME = 'my-app-image'
        DOCKER_IMAGE_TAG = "myuser/my-app:latest"
    }
   stages {
     stage ("checkout") {
      steps {
       checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-cred', url: 'https://github.com/sonyaws501/addressbook.git']])
      }
    }
     stage('Build') {
            steps {
                 sh '/opt/maven/bin/mvn clean package'
            }
        }
     stage('Build Image') {
           steps {
               sh 'whoami'
               sh 'docker build . -t ${DOCKER_IMAGE_TAG}'
            }
        }    
    }
}
