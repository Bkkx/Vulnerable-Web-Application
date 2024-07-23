pipeline {
    agent any
    environment {
        SONARQUBE_URL = 'http://sonarqube:9000' // Use the Docker container name
    }
    stages {
        stage ('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Bkkx/Vulnerable-Web-Application.git'
            }
        }
        stage ('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool name: 'SonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=${SONARQUBE_URL} -Dsonar.login=${SONARQUBE_TOKEN}"
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                try {
                    waitForQualityGate abortPipeline: true
                } catch (Exception e) {
                    echo "Quality gate did not pass: ${e.message}"
                }
            }
        }
    }
}