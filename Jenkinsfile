pipeline {
    agent any

    environment {
        SONARQUBE_SERVER  = 'MySonarQube'   // 🔴 MUST MATCH Jenkins config EXACTLY
        SONAR_PROJECT_KEY = 'sonarqube-test'
        SONAR_CRED_ID     = 'sonar-token'
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/swetha-200160/sonarqube-test.git', branch: 'master'
            }
        }

        stage('Agent Check') {
            steps {
                bat 'java -version'
                bat 'mvn -v'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    withCredentials([string(credentialsId: "${SONAR_CRED_ID}", variable: 'SONAR_TOKEN')]) {
                        bat """
                        mvn sonar:sonar ^
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} ^
                        -Dsonar.login=%SONAR_TOKEN%
                        """
                    }
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
    }

    post {
        success {
            echo '✅ Pipeline succeeded and Quality Gate passed'
        }
        failure {
            echo '❌ Pipeline failed or Quality Gate failed'
        }
    }
}
