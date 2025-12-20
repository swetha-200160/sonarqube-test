pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'
        jdk 'JDK11'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/swetha-200160/sonarqube-test.git'
            }
        }

        stage('Build & Deploy') {
            steps {
                bat 'mvn clean deploy'
            }
        }
    }
}
