pipeline {
    agent any
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    def dockerImage = docker.build('musaele1/ci-cd:latest')
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub and push the image
                    sh "docker login -u musaele1 -p 111P@kistan111"
                    sh "docker push musaele1/ci-cd:latest"
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Load the kubeconfig file
                    withCredentials([file(credentialsId: 'Kubernetes', variable: 'KUBECONFIG')]) {
                        // Set KUBECONFIG environment variable
                        env.KUBECONFIG = "${env.WORKSPACE}/kubeconfig.yaml"
                        
                        // Deploy to Kubernetes using kubectl
                        sh "kubectl apply -f deployment.yaml"
                    }
                }
            }
        }
    }
}
