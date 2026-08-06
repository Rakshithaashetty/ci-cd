pipeline {
    agent any

    environment {
        DOCKER = 'C:/Users/raksh/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                    "%DOCKER%" build --no-cache -t vite-app .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                bat '''
                    "%DOCKER%" stop vite-container || echo Container not running
                    "%DOCKER%" rm vite-container || echo Container not found
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                    "%DOCKER%" run -d -p 8081:80 --name vite-container vite-app
                '''
            }
        }

        stage('Check Container') {
            steps {
                bat '''
                    "%DOCKER%" ps
                '''
            }
        }
    }

    post {
        success {
            echo 'Docker image built and container deployed successfully!'
            echo 'Application: http://localhost:8081'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}