pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
  stages {
    stage('Checkout_Code') {
      steps {
        git(url: 'https://github.com/DadouneDa/spring-boot-examples.git', branch: 'david_dadoune_sol', changelog: true, poll: true, credentialsId: 'dd_github')
        slackSend(channel: 'dd_devops', color: '#3EA652', message: "Success: Stage 'Checkout_Code' on job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

      }
    }

    stage('Compile') {
      steps {
        dir("spring-boot-package-war") {
              echo "${env.BUILD_ID}"
              script {
                def pomFile = 'pom.xml'
                def pom = readMavenPom file: pomFile
                pom.version = "BUILD_${env.BUILD_ID}"
                writeMavenPom file: pomFile, model: pom
              }
              sh 'mvn compile' 
              
            }
        slackSend(channel: 'dd_devops', color: '#3EA652', message: "Success: Stage 'Compile' on job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

      }
    }

    stage('Test') {
      steps {
        sh 'cd spring-boot-package-war && mvn test'
        slackSend(channel: 'dd_devops', color: '#3EA652', message: "Success: Stage 'Test' on job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

      }
    }

    stage('Package') {
      steps {
        dir("spring-boot-package-war") {
        sh 'cd spring-boot-package-war &&  mvn clean package'
        sh 'zip -r package.zip .'
        archiveArtifacts(artifacts: 'package.zip', onlyIfSuccessful: true)
        }
        cleanWs(cleanWhenSuccess: true)
        slackSend(channel: 'dd_devops', color: '#3EA652', message: "Success: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        
      }
    }

  }
}
