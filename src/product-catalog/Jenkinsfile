pipeline {
    agent any

    environment {
        IMAGE_NAME = "manugowda1998"
        APP_PORT = "8088"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/gowdamanu12/ultimate-devops-project-demo.git',
                    branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        echo "Building Docker image..."
                        docker build -t ${IMAGE_NAME}:latest .
                    """
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'dockerhub_creds',
                                     usernameVariable: 'DOCKER_USER',
                                     passwordVariable: 'DOCKER_PASS')
                ]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh """
                        docker push ${IMAGE_NAME}:latest
                    """
                }
            }
        }
    }

    post {
        always {
            sh "docker logout"
        }
    }
}
