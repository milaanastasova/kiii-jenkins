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
                    // Define app globally
                    def app = docker.build("milaanastasova/kiii-jenkins")
                    // Set the app variable as environment variable to make it accessible across stages
                    env.app = app
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        // Access app using the env variable
                        env.app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        env.app.push("${env.BRANCH_NAME}-latest")
                        // signal the orchestrator that there is a new version
                    }
                }
            }
        }
    }
}
