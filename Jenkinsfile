pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
  stages {
    stage('checkout_code') {
      steps {
        git(url: 'https://github.com/DadouneDa/spring-boot-examples.git', branch: 'david_dadoune_sol', changelog: true, poll: true, credentialsId: 'dd_github')
      }
    }

    stage('compile') {
      steps {
        dir("spring-boot-package-war") {
              echo "${env.BUILD_ID}"
              def pomFile = 'pom.xml'
              def pom = readMavenPom file: pomFile
              pom.version = "BUILD_${env.BUILD_ID}"
              writeMavenPom file: pomFile, model: pom
              sh 'mvn compile' 
            }
        /*
        sh 'cd spring-boot-package-war'       
        echo "${env.BUILD_ID}"
        sh 'cd spring-boot-package-war && mvn compile' */
        
      }
    }

    stage('Test') {
      steps {
        sh 'cd spring-boot-package-war && mvn test'
      }
    }

    stage('Package') {
      steps {
        sh 'cd spring-boot-package-war &&  mvn clean package'
        //cleanWs(cleanWhenSuccess: true)
        slackSend()
      }
    }

  }
}
