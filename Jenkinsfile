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
                    withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        docker.withRegistry('https://index.docker.io/v1/', DOCKER_USERNAME, DOCKER_PASSWORD) {
                            docker.image('musaele1/ci-cd:latest').push()
                        }
                    }
                }
            }
        }
    }
}
