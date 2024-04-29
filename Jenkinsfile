pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Checkout the source code from your Git repository
                git 'https://github.com/Musaele/CI-CD.git'
                
                // Use Node.js Docker image for building
                docker.image('node:latest').inside {
                    // Install dependencies
                    sh 'npm install'
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
        
        stage('Deploy') {
            steps {
                // Build the Docker image with the appropriate tag
                sh 'docker build -t musaele1/ci-cd:latest .'
                
                // Push the Docker image to the repository
                sh 'docker push musaele1/ci-cd:latest'
            }
        }
    }
}
