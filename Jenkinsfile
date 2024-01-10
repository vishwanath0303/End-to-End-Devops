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
                   withDockerRegistry(credentialsId: '82c1e202-d6df-47f2-8ea3-377fb7929124', toolName: 'docker-latest') {
                       sh 'docker build -t my-app:$BUILD_NUMBER .'

                }
            }
     }
  }
      stage('Start image'){
            steps{
                 script{
                   withDockerRegistry(credentialsId: '82c1e202-d6df-47f2-8ea3-377fb7929124', toolName: 'docker-latest') {
                     sh 'docker run -d -p 8181:8181 --name spring spring:$BUILD_NUMBER '
                 }
             }
                }
            }
       
  
     stage('Start image'){
            steps{
                 script{
                   withDockerRegistry(credentialsId: '82c1e202-d6df-47f2-8ea3-377fb7929124', toolName: 'docker-latest') {
                     sh "docker tag my-app vkulkarni0303/my-app:$BUILD_NUMBER "
                     sh "docker push $vkulkarni0303/my-app:$BUILD_NUMBER"
                 }
             }
     }
        
  }
}
}
