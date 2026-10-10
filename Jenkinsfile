
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
                powershell '''
                    & "C:/Users/AKSHAYA JEYAGOPAL/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe" build -t flask-calculator .
                    if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }
                '''
            }
        }

        stage('Verify Docker Image') {
            steps {
                powershell '''
                    & "C:/Users/AKSHAYA JEYAGOPAL/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe" image inspect flask-calculator
                    if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                powershell '''
                    $docker = "C:/Users/AKSHAYA JEYAGOPAL/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe"

                    & $docker rm -f flask-calculator-container 2>$null
                    & $docker run -d --name flask-calculator-container -p 5000:5000 --restart unless-stopped flask-calculator:latest

                    if ($LASTEXITCODE -ne 0) {
                        throw "Docker container deployment failed."
                    }
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                powershell '''
                    $docker = "C:/Users/AKSHAYA JEYAGOPAL/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe"

                    Start-Sleep -Seconds 5
                    & $docker ps --filter "name=flask-calculator-container"

                    if ($LASTEXITCODE -ne 0) {
                        throw "Could not verify the deployed container."
                    }
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Flask Calculator built, verified, and deployed.'
        }
        failure {
            echo 'FAILED: Check the Jenkins console output.'
        }
    }
}
