pipeline {
    agent any
    environment {
        SONARQUBE_URL = 'http://sonarqube:9000'
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
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=${SONARQUBE_URL}"
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