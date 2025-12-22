pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
    }

    tools {
        maven 'MAVEN_HOME'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/swetha-200160/sonarqube-test.git'
            }
        }

        stage('Build') {
            steps {
                bat 'java -version'
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    bat '''
                    mvn sonar:sonar ^
                    -Dsonar.projectKey=sonarqube-test ^
                    -Dsonar.projectName=sonarqube-test
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                bat 'mvn deploy'
            }
        }
    }
}
