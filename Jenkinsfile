pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/your/repo.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('sonarqube') {
          sh 'mvn sonar:sonar'
        }
      }
    }

    stage('Quality Gate') {
      steps {
        waitForQualityGate abortPipeline: true
      }
    }

    stage('Deploy to Nexus') {
      steps {
        sh 'mvn deploy'
      }
    }

    stage('Docker Build & Push') {
      steps {
        sh '''
        docker build -t myapp:1.0 .
        docker tag myapp:1.0 localhost:8082/myapp:1.0
        docker push localhost:8082/myapp:1.0
        '''
      }
    }
  }
}
