pipeline {
  agent any

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/yourname/java-consumer-app.git'
      }
    }

    stage('Build (Pulls from Nexus)') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t consumer-app:1.0 .'
      }
    }
  }
}
