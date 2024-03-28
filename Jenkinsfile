def app

pipeline {
    agent any

    stages {
        // Define other stages here

        stage('Build image') {
            steps {
                script {
                    // Build the Docker image and assign it to the 'app' variable
                    app = docker.build("milaanastasova/kiii-jenkins")
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        // Access the 'app' variable and push the Docker image
                        app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        app.push("${env.BRANCH_NAME}-latest")
                    }
                }
            }
        }
    }
}
