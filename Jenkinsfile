pipeline {
    agent any
    environment {
        SONARQUBE_URL = 'http://sonarqubelab9:9000'
    }
    stages {
        stage ('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/bkkx/Vulnerable-Web-Application.git'
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
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}