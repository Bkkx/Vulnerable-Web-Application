pipeline {
    agent any

    environment {
        SONARQUBE_TOKEN = credentials('sonarqube') // Using the ID 'sonarqube' for the secret text token
    }

    tools {
        // Correct tool type for SonarQube
        sonarScanner 'SonarQube'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/Bkkx/Vulnerable-Web-Application.git'
            }
        }

        stage('Code Quality Check via SonarQube') {
            environment {
                scannerHome = tool name: 'SonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
            }
            steps {
                withSonarQubeEnv('SonarQube') { 
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${SONARQUBE_TOKEN}"
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
                    echo "Quality Gate failed: ${e.message}"
                }
            }
        }
    }
}