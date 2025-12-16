pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your/repo.git',
                    credentialsId: 'git-credentials'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                bat 'mvn deploy'
            }
        }

        stage('Docker Build & Push') {
            steps {
                bat 'docker build -t myapp:latest .'
            }
        }
    }
}
