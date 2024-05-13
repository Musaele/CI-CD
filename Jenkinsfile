pipeline {
    agent any
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build('musaele1/ci-cd:latest')
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "docker login -u musaele1 -p 111P@kistan111"
                    sh "docker push musaele1/ci-cd:latest"
                }
            }
        }
    }
}
