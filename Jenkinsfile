pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                powershell 'docker build -t flask-calculator .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                powershell 'docker image inspect flask-calculator'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Flask Calculator Docker image built successfully.'
        }
        failure {
            echo 'FAILED: Check the Jenkins console output for details.'
        }
    }
}