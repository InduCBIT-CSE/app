pipeline {
    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/sample-web-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from GitHub repository
                git branch: 'main',
                    url: 'https://github.com/youruser/sample-web-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    sh """
                        docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Docker Login') {
            steps {
                // Login to Docker Hub (using credentials stored in Jenkins)
                script {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                // Push the Docker image to DockerHub (optional step)
                script {
                    sh """
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Run Container') {
            steps {
                // Run the container to verify it works
                script {
                    sh """
                        docker run -d --name test-app -p 5000:5000 ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
    }

    post {
        always {
            // Cleanup the container after running
            echo "Cleaning up..."
            sh "docker stop test-app || true"
            sh "docker rm test-app || true"
        }
    }
}
