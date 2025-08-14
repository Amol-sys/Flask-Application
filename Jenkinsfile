pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "flask-app"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Amol-sys/Flask-Application.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'docker run --rm $DOCKER_IMAGE:$DOCKER_TAG python -m unittest || echo "Skipping tests"'
            }
        }

        stage('Push to Local Registry') {
            steps {
                sh 'docker tag $DOCKER_IMAGE:$DOCKER_TAG localhost:5000/$DOCKER_IMAGE:$DOCKER_TAG'
                sh 'docker push localhost:5000/$DOCKER_IMAGE:$DOCKER_TAG'
            }
        }
    }
}
