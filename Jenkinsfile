pipeline {
  agent any 
  tools {
      jdk 'jdk11'
      maven 'maven3'
  }
   environment{
        SCANNER_HOME= tool 'Sonarqube'
    }
  
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/vishwanath0303/spring-petclinic-jenkins-pipeline.git'
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
        stage('Sonar Analysis') {
            steps {
               withSonarQubeEnv('Sonarqube'){
               sh "mvn clean package sonar:sonar  -Dsonar.exclusions=src/main/**/*.java"
               sh "$SCANNER_HOME/target/sonar "
            }
        }
        }
  }
}
