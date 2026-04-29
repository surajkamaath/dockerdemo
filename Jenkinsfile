pipeline {
    agent any

    environment {
        IMAGE_NAME = "surajkamathh/myapp"
        IMAGE_TAG = "v1"
    }

    stages {
        stage('Docker Check') {
            steps {
                bat "whoami"
                bat "docker --version"
                bat "docker info"
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    bat "echo %DOCKER_TOKEN% | docker login -u %DOCKER_USER% --password-stdin"
                }
            }
        }

        stage('Pull Base Image') {
            steps {
                bat "docker pull python:3.10-slim"
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Run Docker Image') {
            steps {
                bat "docker run --rm %IMAGE_NAME%:%IMAGE_TAG%"
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
            }
        }

        stage('Cleanup') {
            steps {
                bat "docker system prune -f"
            }
        }
    }
}
