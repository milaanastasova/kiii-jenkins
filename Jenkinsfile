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
                    // Define the 'app' variable at the pipeline level
                    env.APP_IMAGE = docker.build("milaanastasova/kiii-jenkins")
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        // Use the 'APP_IMAGE' variable defined at the pipeline level
                        env.APP_IMAGE.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        env.APP_IMAGE.push("${env.BRANCH_NAME}-latest")
                        // signal the orchestrator that there is a new version
                    }
                }
            }
        }
    }
