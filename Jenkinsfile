pipeline {
    agent any

    environment {
        DOCKER_HUB = credentials('hub') // Store in Jenkins credentials
        IMAGE_NAME = "ajaykumara/zapbook"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/ajaykumarbk/zapbook.git'
                // Removed trailing comma that was causing the syntax error
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'hub') {
                        docker.image("${IMAGE_NAME}:${env.BUILD_NUMBER}").push()
                        // Optional: Push as 'latest'
                        docker.image("${IMAGE_NAME}:${env.BUILD_NUMBER}").push('latest')
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace
        }
    }
}
