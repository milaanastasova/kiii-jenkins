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
                    // Check if the Docker image is built successfully
                    def dockerImage = docker.image("milaanastasova/kiii-jenkins")
                    if (dockerImage != null) {
                        // Push the Docker image if it's not null
                        dockerImage.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        dockerImage.push("${env.BRANCH_NAME}-latest")
                        // Signal the orchestrator that there is a new version
                    } else {
                        error "Docker image not found or failed to build"
                    }
                }
            }
        }
    }
}
