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
                    docker.withRegistry('https://index.docker.io/v1/', 'musaele1', '111P@kistan111') {
                        docker.image('musaele1/ci-cd:latest').push()
                    }
                }
            }
        }
    }
}
