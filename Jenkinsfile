pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'MAVEN_HOME'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/swetha-200160/sonarqube-test.git',
                    branch: 'master'
            }
        }

        stage('Build & Deploy to Nexus') {
            steps {
                bat '''
                java -version
                mvn -version
                mvn clean deploy -DskipTests
                '''
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
            echo '✅ Build, Nexus deploy, and SonarQube analysis SUCCESS'
        }
        failure {
            echo '❌ Pipeline FAILED'
        }
    }
}
