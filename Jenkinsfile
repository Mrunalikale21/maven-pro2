pipeline {
    agent any
    tools{
      maven 'sonarmaven'
    }
    environment{
      SONAR_TOKEN = credentials('sonar-token')
    }
    stages{
      stage('Checkout'){
        steps{
          checkout scm
        }
      }
      stage('Build'){
        steps{
          bat 'mvn clean package'
        }
      }
      stage('SonarQube Anaylsis'){
        steps{
          withSonarqubeEnv('sonarqube'){
            bat """
              mvn sonar:sonar \
              -Dsonar.projectKey=maven-pro2 \
              -Dsonar.sources=src/main/java \
              -Dsonar.host.url=http://localhost:9000 \
              -Dsonar.login=%SONAR_TOKEN%
            """
          }
        }
      }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
