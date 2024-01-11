pipeline {
  agent any 
  tools {
      jdk 'jdk11'
      maven 'maven3'
  }
  
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/vishwanath0303/spring-petclinic-jenkins-pipeline1.git'
      }
    }
    stage('mvn build') {
      steps {
        sh '''
        mvn clean install
        ls
        pwd
        ''' 
        //if the code is compiled, we test and package it in its distributable format; run IT and store in local repository
      }
    }
     stage('Build docker image'){
            steps{
                script{
                       sh 'docker build -t my-app:$BUILD_NUMBER .'

                
            }
       }
      }
    stage ("Stop and remove") {
            steps {
              script{
                sh 'docker ps -a -q'
                sh 'docker stop my-app '
                sh 'docker rm my-app '
              }
            }
         }
  
      stage('Start image'){
            steps{
                 script{
                  
                     sh 'docker run -d -p 8181:8181 --name my-app my-app:$BUILD_NUMBER '          
             }
           }
          }
       
    stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-app:$BUILD_NUMBER ${env.dockerHubUser}/my-app:$BUILD_NUMBER"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-app:$BUILD_NUMBER"
                }
            }
        }
       
  }
}
