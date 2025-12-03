pipeline {
    agent any

    environment {
        IMAGE_NAME = "sample-web-app" 
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from GitHub repository using the updated credentials
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/InduCBIT-CSE/app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Run Container') {
            steps {
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
            echo "Cleaning up..."
            sh "docker stop test-app || true"
            sh "docker rm test-app || true"
        }
    }
}

