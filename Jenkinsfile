pipeline {
  agent any

  environment {
    SONARQUBE_SERVER  = 'Sonar'
    SONAR_PROJECT_KEY = 'sonarqube-test'
    SONAR_CRED_ID     = 'sonar-token'
}


  stages {
    stage('Checkout') {
      steps { git branch: "${GIT_BRANCH}", url: "${GIT_REPO_URL}" }
    }

    stage('Agent check') {
      steps {
        bat 'java -version || echo java_not_found'
        bat 'mvn -v || echo maven_not_found'
      }
    }

    stage('Find POM & Build') {
      steps {
        script {
          // find first pom and run build (same as before)
          def rc = bat(returnStatus: true, script: '''
@echo off
set "POM="
for /f "delims=" %%F in ('dir /s /b pom.xml 2^>nul') do (
  set "POM=%%~fF"
  goto FOUND
)
echo ERROR: pom.xml not found
exit /b 2
:FOUND
set "POM_DIR=%%~dpF"
set "POM_DIR=%POM_DIR:~0,-1%"
cd /d "%POM_DIR%"
mvn -B -DskipTests clean package
''')
          if (rc != 0) {
            // run debug and archive (same pattern)
            bat """
@echo off
cd /d "%POM_DIR%"
mvn -X clean package > "${env.WORKSPACE}\\${DEBUG_LOG}" 2>&1
"""
            archiveArtifacts artifacts: "${DEBUG_LOG}", allowEmptyArchive: false
            error("Maven build failed. Debug log archived.")
          }
        }
      }
      post { always { junit testResults: '**\\target\\surefire-reports\\*.xml', allowEmptyResults: true } }
    }

   stage('SonarQube Analysis') {
    steps {
        script {
            bat '''
@echo off
set "POM_DIR="
for /f "delims=" %%F in ('dir /s /b pom.xml 2^>nul') do (
  set "POM_DIR=%%~dpF"
  goto FOUND
)
echo ERROR: pom.xml not found
exit /b 2
:FOUND
set "POM_DIR=%POM_DIR:~0,-1%"
cd /d "%POM_DIR%"
'''
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
}



    stage('Quality Gate') {
    steps {
        timeout(time: 5, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}

  post {
    success { echo '✅ Pipeline succeeded and Quality Gate passed' }
    failure { echo '❌ Pipeline failed or Quality Gate failed' }
    always { archiveArtifacts artifacts: "${DEBUG_LOG}", allowEmptyArchive: true }
  }
}
