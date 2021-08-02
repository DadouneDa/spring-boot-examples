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
        sh 'cd spring-boot-package-war && mvn compile'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }

    stage('Package') {
      steps {
        sh ' mvn clean package'
        cleanWs(cleanWhenSuccess: true)
        slackSend()
      }
    }

  }
}