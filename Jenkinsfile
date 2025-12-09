pipeline {
    agent any

    environment {
        BACKEND_IMAGE = "mern-backend:jenkins"
        FRONTEND_IMAGE = "mern-frontend:jenkins"
        PORT = "5000"
        MONGO_URI = "mongodb://mongo:27017/taskdb"
    }

    stages {

        stage('Checkout code') {
            steps {
                git url: 'https://github.com/jhabineet/devops-learning.git', branch: 'main'
            }
        }

        stage('Prepare .env') {
            steps {
                sh '''
                    cat > server/.env <<EOF
                    PORT=$PORT
                    MONGO_URI=$MONGO_URI
                    EOF
                '''
            }
        }

        stage('Build Docker images') {
    steps {
        sh '''
            echo "Building backend image..."
            docker build -t $BACKEND_IMAGE ./server

            echo "Building frontend image..."
            docker build -t $FRONTEND_IMAGE ./client --build-arg VITE_API_URL=http://localhost:5000/api
        '''
    }
}

stage('Run docker compose') {
    steps {
        sh '''
            echo "Starting MERN app..."
            docker compose up -d --no-build

            echo "Showing containers..."
            docker compose ps

            echo "=====Backend logs======="
            docker compose logs backend || true

            echo "=====Frontend logs======="
            docker compose logs frontend || true
        '''
    }
}

    }
}
