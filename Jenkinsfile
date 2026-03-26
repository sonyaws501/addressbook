pipeline {
   agent any
   environment {
        IMAGE_NAME = 'your-dockerhub-user/your-app'
    }
//   tools {
//        // 'M3' must match the name configured in Manage Jenkins -> Global Tool Configuration
//        maven 'maven3' 
    }
   stages {
     stage ("checkout") {
      steps {
       checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-cred', url: 'https://github.com/sonyaws501/addressbook.git']])
      }
    }
      stage('Build') {
            steps {
                 sh 'mvn clean package'
            }
        }
//       stage('Build Image') {
//            steps {
//                script { dockerImage = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}") }
//            }
//        }    
}
}
