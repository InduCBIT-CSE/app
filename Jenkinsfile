pipeline {
    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/sample-web-app"  // Update with your DockerHub username
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from GitHub repository using credentials
                git credentialsId: 'github-credentials', url: 'https://github.com/InduCBIT-CSE/app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image from the Dockerfile
                    sh """
                        docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    // Docker login using credentials (ensure DOCKER_USERNAME and DOCKER_PASSWORD are set as environment variables in Jenkins)
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                script {
                    // Push the Docker image to DockerHub (optional)
                    sh """
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Run the Docker container to test the app
                    sh """
                        docker run -d --name test-app -p 5000:5000 ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
    }

    post {
        always {
            // Cleanup the container after the job is finished
            echo "Cleaning up..."
            sh "docker stop test-app || true"
            sh "docker rm test-app || true"
        }
    }
}
