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
                    docker.build("milaanastasova/kiii-jenkins")
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        // Use withCredentials to securely pass Docker credentials
                        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            // Login to Docker Hub
                            docker.login(username: DOCKER_USERNAME, password: DOCKER_PASSWORD)
                            // Push the image to Docker Hub
                            docker.image("milaanastasova/kiii-jenkins").push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                            docker.image("milaanastasova/kiii-jenkins").push("${env.BRANCH_NAME}-latest")
                        }
                    }
                }
            }
        }
    }
}
