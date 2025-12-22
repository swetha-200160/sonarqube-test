pipeline {
    agent any
 
    tools {
        maven 'maven'
    }
 
    environment {
        SONARQUBE_ENV = 'SonarQubeServer'
        DEPLOY_PATH = 'C:\Users\swethasuresh\sample file'
        JAR_NAME = 'hrms.jar'
    }
 
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', 
                    url: 'https://github.com/swetha-200160/sonarqube-test.git', 
                    credentialsId: 'gitcred'
            }
        }
 
        stage('Build Project') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }
 
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    bat 'mvn sonar:sonar'
                }
            }
        }
 
        stage('Copy JAR to Deploy Path') {
            steps {
                script {
                    bat """
                        if not exist "${DEPLOY_PATH}" mkdir "${DEPLOY_PATH}"
                        copy target\\*.jar "${DEPLOY_PATH}\\${JAR_NAME}" /Y
                    """
                }
            }
        }
    }
 
    post {
        success {
            echo 'Build, SonarQube Analysis, and JAR copy completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}