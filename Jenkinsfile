pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Hariveerj/pickles.git'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'SonarScanner' // must match your configured name
            }
            steps {
                withSonarQubeEnv('SonarQube') { // must match the name in Manage Jenkins > System
                    sh """
                    ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=pickle \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://54.173.134.215:9000
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
