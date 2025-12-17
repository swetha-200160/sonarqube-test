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
                    url: 'https://github.com/swetha-200160/java-project.git',
                    credentialsId: 'git-credentials'
            }
        }

        stage('Build') {
    steps {
        dir('my-app') {
            bat 'mvn clean compile'
        }
    }
}

stage('SonarQube Analysis') {
    steps {
        dir('my-app') {
            withSonarQubeEnv('SonarQube') {
                bat 'mvn sonar:sonar'
            }
        }
    }
}

stage('Deploy to Nexus') {
    steps {
        dir('my-app') {
            bat 'mvn deploy'
        }
    }
}


        stage('Docker Build & Push') {
            steps {
                bat 'docker build -t myapp:latest .'
            }
        }
    }
}
