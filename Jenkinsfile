pipeline {
  agent any
  environment {
       imagename = "pilas01/pilascloud"
       registryCredential = 'DockerHub'
       dockerImage = ''
           }
  tools {
    maven 'maven-integration' 
       }
  stages {
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Building Docker image') {
          steps{
                script {
                     dockerImage = docker.build imagename
                          }
                      }
                }
     stage('Deploy Image') {
           steps{
               script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                                              }
                                    }
                             }
                  }
     stage('Remove Unused docker image') {
          steps{
              sh "docker rmi $imagename:$BUILD_NUMBER"
              sh "docker rmi $imagename:latest"
                        }
            }
    stage ('Deploy To Tomcat Server') {
      steps{
        script {
         deploy adapters: [tomcat9(credentialsId: '3131a1c5-886e-4207-a3f7-f5b113410434', path: '', url: 'http://18.208.165.21:8080')], contextPath: 'hello-world', war: '**/*.war'
      }
     }
   }
 }
}

