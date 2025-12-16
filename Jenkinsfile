pipeline {
  agent any


    stage('Checkout') {
    steps {
        git branch: 'main',
            url: 'https://github.com/your/repo.git',
            credentialsId: 'git-credentials'
    }
}


    stage('Build') {
      steps {
        bat 'mvn clean package'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('sonarqube') {
          bat 'mvn sonar:sonar'
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
        bat 'mvn deploy'
      }
    }

    stage('Docker Build & Push') {
      steps {
        bat '''
        docker build -t myapp:1.0 .
        docker tag myapp:1.0 localhost:8082/myapp:1.0
        docker push localhost:8082/myapp:1.0
        '''
      }
    }
  }
}
