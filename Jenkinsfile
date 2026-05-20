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
            // Get the path to the sonar-scanner binary
            def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
            sh """
                ${scannerHome}/bin/sonar-scanner \
                  -Dsonar.projectKey=python-sonarqube-pipeline \
                  -Dsonar.sources=. \
                  -Dsonar.python.version=3
            """
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
}
