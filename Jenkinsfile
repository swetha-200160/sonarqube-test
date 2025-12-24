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
    bat 'mvn clean verify'
}

            }
        }

       stage('Deploy to Nexus') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'nexus-creds',
            usernameVariable: 'NEXUS_USER',
            passwordVariable: 'NEXUS_PASS'
        )]) {
            bat """
            curl -u %NEXUS_USER%:%NEXUS_PASS% ^
            --upload-file target/hrms-1.0.1.jar ^
            http://localhost:8081/repository/maven-releases/com/company/hrms/1.0.1/hrms-1.0.1.jar
            """
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
