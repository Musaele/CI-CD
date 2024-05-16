pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS = credentials('docker')
    }
    
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
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                        sh "docker push musaele1/ci-cd:latest"
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'Kubernetes', variable: 'KUBECONFIG')]) {
                        sh "microk8s.kubectl --kubeconfig=$KUBECONFIG apply -f deployment.yaml"
                    }
                }
            }
        }
    }
}
