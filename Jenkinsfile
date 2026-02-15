pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-php-app"
        CONTAINER_NAME = "php-container"
        // Docker daemon over TCP
        DOCKER_HOST = "tcp://localhost:2375"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                echo "🔹 Building Docker image..."
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Stop Old Container') {
            steps {
                echo "🔹 Stopping old container (if exists)..."
                bat '''
                docker stop %CONTAINER_NAME% 2>nul
                docker rm %CONTAINER_NAME% 2>nul
                exit 0
                '''
            }
        }

        stage('Run Container') {
            steps {
                echo "🔹 Running new container..."
                bat 'docker run -d -p 8080:80 --name %CONTAINER_NAME% %IMAGE_NAME%'
            }
        }

        stage('Verify Running') {
            steps {
               echo "🔹 Verifying running containers..."
               bat 'docker ps'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Pipeline Failed!'
        }
    }
}
