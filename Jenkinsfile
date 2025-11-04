pipeline {
    agent any

    environment {
        // Jenkins credentials ID for DockerHub login
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')

        // Your DockerHub username
        DOCKERHUB_USER = 'koushikbijili'

        // Docker image names
        BACKEND_IMAGE = "${DOCKERHUB_USER}/movie-app-backend"
        FRONTEND_IMAGE = "${DOCKERHUB_USER}/movie-app-frontend"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "📦 Cloning repository from GitHub..."
                git branch: 'main', url: 'https://github.com/koushikbijili/movie-app.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                echo "🐳 Building backend and frontend Docker images..."
                sh 'docker build -t $BACKEND_IMAGE:latest ./backend'
                sh 'docker build -t $FRONTEND_IMAGE:latest ./frontend'
            }
        }

        stage('Push Images to DockerHub') {
            steps {
                echo "📤 Pushing Docker images to DockerHub..."
                sh '''
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                    docker push $BACKEND_IMAGE:latest
                    docker push $FRONTEND_IMAGE:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "☸️ Deploying application to Kubernetes..."
                sh '''
                    kubectl apply -f k8s/backend-deployment.yaml
                    kubectl apply -f k8s/frontend-deployment.yaml
                    kubectl get pods -o wide
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs for errors."
        }
    }
}


