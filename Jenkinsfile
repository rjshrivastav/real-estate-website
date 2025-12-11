pipeline {
    agent any

    environment {
        IMAGE_NAME = "real-estate-app"
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
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh "docker rm -f real-estate-app-container || true"
                    sh "docker run -d --name real-estate-app-container -p 3000:3000 ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}
