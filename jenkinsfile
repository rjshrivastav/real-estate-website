pipeline {
    agent any

    environment {
        IMAGE_NAME = "real-estate-app"      // change if you want
        IMAGE_TAG  = "build-${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    echo "Building Docker image ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh """
                    docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Run Tests (optional)') {
            when {
                expression { return false } // set to true when you add tests
            }
            steps {
                script {
                    echo "Running tests inside container"
                    sh """
                    docker run --rm ${IMAGE_NAME}:${IMAGE_TAG} npm test
                    """
                }
            }
        }

        stage('Run Container (optional)') {
            when {
                expression { return false } // set to true if you want Jenkins to run it
            }
            steps {
                script {
                    echo "Starting container from image ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh """
                    docker rm -f real-estate-app-container || true
                    docker run -d --name real-estate-app-container -p 3000:3000 ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        // Example stage if you later want to push to Docker Hub or ECR
        stage('Push to Registry (optional)') {
            when {
                expression { return false } // flip to true when you configure registry
            }
            steps {
                script {
                    echo "Tagging and pushing image"
                    sh """
                    docker tag ${IMAGE_NAME}:${IMAGE_TAG} your-docker-user/${IMAGE_NAME}:${IMAGE_TAG}
                    docker login -u your-docker-user -p your-docker-password
                    docker push your-docker-user/${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Jenkins pipeline completed successfully"
        }
        failure {
            echo "❌ Jenkins pipeline failed – check the logs"
        }
    }
}
