pipeline {
    options {
        skipDefaultCheckout true
    }
    agent any

    environment {
        // Define Docker Hub credentials ID
        DOCKER_HUB_CREDENTIALS = 'dockerhub_credentials'
    }
    
    // Increase logging level for troubleshooting
    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '5'))
        timestamps()
        timeout(time: 1, unit: 'HOURS')
        buildRetention(daysToKeepStr: '30')
        disableConcurrentBuilds()
        skipDefaultCheckout()
        retry(3)
        disableResume()
        parallelsAlwaysFailFast()
    }

    stages {
        stage('Build') {
            steps {
                // Checkout the source code from your Git repository
                git branch: 'main', url: 'https://github.com/Musaele/CI-CD.git'

                // Build the Docker image
                script {
                    dockerImage = docker.build('musaele1/ci-cd:latest')
                }
            }
        }
        
        stage('Test') {
            steps {
                // Run tests if any
                // Replace the following command with your test command
                sh 'npm test'
            }
        }
        
        stage('Deploy to Docker Hub') {
            steps {
                // Authenticate with Docker Hub
                withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Log in to Docker Hub
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'

                    // Push the Docker image to Docker Hub
                    sh 'docker push musaele1/ci-cd:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            environment {
                // Define Kubernetes credentials ID
                KUBE_CONFIG = credentials('Kubernetes')
            }
            steps {
                // Set up Kubernetes configuration
                withKubeConfig(credentialsId: KUBE_CONFIG) {
                    // Apply Kubernetes deployment
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}
