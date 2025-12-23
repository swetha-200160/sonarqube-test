pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'MAVEN_HOME'
    }

    stages {

        stage('Pull Code') {
            steps {
                git url: 'https://github.com/swetha-200160/sonarqube-test.git',
                    branch: 'master'
            }
        }

        stage('Build Project') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    bat 'mvn verify sonar:sonar'
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                bat 'mvn deploy -DskipTests'
            }
        }
    }

    post {
        success {
            echo '✅ Pull → Build → Sonar → Nexus SUCCESS'
        }
        failure {
            echo '❌ Pipeline FAILED'
        }
    }
}
