pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'MAVEN_HOME'
    }

    environment {
        JAVA_HOME = tool(name: 'JDK21', type: 'jdk')
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
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
                bat 'echo JAVA_HOME=%JAVA_HOME%'
                bat 'java -version'
                bat 'mvn -version'
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
            echo '✅ Build + Nexus deploy + SonarQube successful'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}
