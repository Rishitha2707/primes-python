pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') { 
                    sh 'sonar-scanner -Dsonar.projectKey=python-sonarqube-pipeline -Dsonar.sources=. -Dsonar.python.version=3'
                }
            }
        }

        stage('Quality Gate Result') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    // The pipeline pauses here, waiting for the webhook to deliver the result.
                    waitForQualityGate abortPipeline: false 
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed: Quality Gate passed!'
        }
        failure {
            echo 'Pipeline failed: Quality Gate check returned an error or failed.'
        }
    }
}
