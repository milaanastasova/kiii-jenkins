pipeline {
    agent any

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                script {
                    // Build the Docker image
                    dockerImage = docker.build("milaanastasova/kiii-jenkins")
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerImage.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        dockerImage.push("${env.BRANCH_NAME}-latest")
                    }
                }
            }
        }
    }
}
