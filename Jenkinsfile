pipeline {
    agent any

    environment {
        DOCKER_HUB = 'amol297'
        IMAGE_NAME = 'flask-k8s-app'
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Amol-sys/Flask-Application.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $DOCKER_HUB/$IMAGE_NAME:latest ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'Dockerhub-cre', url: 'https://index.docker.io/v1/']) {
                        sh "docker push $DOCKER_HUB/$IMAGE_NAME:latest"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f k8s-deployment.yaml"
                    sh "kubectl apply -f k8s-service.yaml"
                }
            }
        }
    }
}

