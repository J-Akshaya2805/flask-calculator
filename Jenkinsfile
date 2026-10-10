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
                powershell '& "C:\Users\AKSHAYA JEYAGOPAL\AppData\Local\Programs\DockerDesktop\resources\bin\docker.exe" build -t flask-calculator .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                powershell '& "C:\Users\AKSHAYA JEYAGOPAL\AppData\Local\Programs\DockerDesktop\resources\bin\docker.exe" image inspect flask-calculator'
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