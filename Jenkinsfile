pipeline {
    agent any

    environment {
        APP_NAME = 'Insightify-App'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Fetching latest code from GitHub...'
                checkout scm
            }
        }

        stage('Stop Existing Containers') {
            steps {
                echo 'Stopping old containers to avoid port conflicts...'
                sh 'docker compose down || true'
            }
        }

        stage('Build & Deploy Services') {
            steps {
                echo 'Building images and starting Docker Compose containers...'
                sh 'docker compose up -d --build'
            }
        }

        stage('Verify Containers Status') {
            steps {
                echo 'Checking running containers...'
                sh 'docker ps --filter "name=insightify-app"'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful! Insightify-App containers are up and running.'
        }
        failure {
            echo 'Deployment failed! Checking Docker logs...'
            sh 'docker compose logs --tail=50'
        }
    }
}
