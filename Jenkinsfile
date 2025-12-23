pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'
        jdk 'JDK11'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/swetha-200160/sonarqube-test.git'
            }
        }

        stage('Build & Deploy to Nexus') {
            steps {
                bat 'java -version'
                bat 'mvn clean deploy -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    bat 'mvn sonar:sonar'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, SonarQube analysis, and Nexus deploy successful.'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
