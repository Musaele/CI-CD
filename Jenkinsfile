pipeline {
    agent any

    environment {
        // Define Docker Hub credentials ID
        DOCKER_HUB_CREDENTIALS = 'docker-credentials'
        // Define the path to the kubeconfig file
        KUBECONFIG = '/var/lib/jenkins/workspace/CI-CD/kubeconfig.yaml'
    }

    stages {
        stage('Build and Push Docker Image') {
            steps {
                // Checkout the source code from your Git repository
                git branch: 'main', url: 'https://github.com/Musaele/CI-CD.git'

                // Build the Docker image
                script {
                    dockerImage = docker.build('musaele1/ci-cd:latest')
                }
                
                // Authenticate with Docker Hub and push the Docker image
                withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Log in to Docker Hub
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'

                    // Push the Docker image to Docker Hub
                    sh 'docker push musaele1/ci-cd:latest'
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                // Set the KUBECONFIG environment variable
                withEnv(["KUBECONFIG=${KUBECONFIG}"]) {
                    // Apply the Deployment YAML file
                    sh 'kubectl apply -f deployment.yaml'
                    
                    // Apply the Service YAML file
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }
}
