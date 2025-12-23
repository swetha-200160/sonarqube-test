pipeline {
    agent any
 
    tools {
        maven 'MAVEN_HOME'
    }
 
    environment {
        SONARQUBE_ENV = 'SonarQubeServer'
        DEPLOY_PATH   = 'C:/Users/swethasuresh/sample file'
        JAR_NAME      = 'hrms.jar'
    }
 
    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/swetha-200160/sonarqube-test.git',
                    credentialsId: 'gitcred'
            }
        }
 stage('Build & Deploy to Nexus') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'nexus-cred',
            usernameVariable: 'admin',
            passwordVariable: 'b8c6bf02-b663-4a5b-8ef3-a2618a625c90'
        )]) {
            bat 'mvn clean deploy -DskipTests -Dnexus.username=%NEXUS_USER% -Dnexus.password=%NEXUS_PASS%'
        }
    }
}





 
       stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('SonarQubeServer') {
            bat 'mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar'
        }
    }
}

 
        stage('Copy JAR to Deploy Path') {
            steps {
                bat """
                if not exist "${DEPLOY_PATH}" mkdir "${DEPLOY_PATH}"
                copy target\\*.jar "${DEPLOY_PATH}\\${JAR_NAME}" /Y
                """
            }
        }
    }
 
    post {
        success {
            echo '✅ Build, SonarQube analysis, and JAR copy completed successfully.'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
