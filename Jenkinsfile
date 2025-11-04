pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-login') // Jenkins Credentials ID
        DOCKERHUB_USER = 'koushikbijili'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/koushikbijili/movie-app.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/movie-backend:latest ./backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/movie-frontend:latest ./frontend'
            }
        }

        stage('Push Images to DockerHub') {
            steps {
                sh '''
                  echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                  docker push $DOCKERHUB_USER/movie-backend:latest
                  docker push $DOCKERHUB_USER/movie-frontend:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                  kubectl apply -f k8s/backend-deployment.yaml
                  kubectl apply -f k8s/frontend-deployment.yaml
                '''
            }
        }
    }
}

