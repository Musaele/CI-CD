pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '5'))
        timestamps()
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
        retry(3)
        parallelsAlwaysFailFast()
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
                
                // Push the Docker image to Docker Hub
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push musaele1/ci-cd:latest'
                }
            }
        }
    }
}
