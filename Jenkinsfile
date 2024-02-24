pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository from GitHub
                git 'https://github.com/git-yasir/jenkins.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build Docker image from Dockerfile
                script {
                    docker.build("yasirdocker/web-app:${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                // Push the Docker image to DockerHub
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials') {
                        docker.image("yasirdocker/web-app:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                // Apply Kubernetes deployment
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
