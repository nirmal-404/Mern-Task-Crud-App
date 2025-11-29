pipeline {
    agent any

    environment {
        FRONTEND_IMAGE= "mern-task-client:jenkins"
        BACKEND_IMAGE= "mern-task-server:jenkins"
        PORT= "5000"
        MONGO_URI= "mongodb://mongo:27017/taskDb"
    }

    stages {
        stage('Checkout Repo') {
            steps {
                git url: 'https://github.com/nirmal-404/Mern-Task-Crud-App.git', branch: 'main'
            }
        }

        stage('Prepare ENV') {
            steps {
        sh '''
            mkdir -p server
            cat > server/.env <<EOF
PORT=${PORT}
MONGO_URI=${MONGO_URI}
EOF
        '''
    }
}

        stage('Build Docker Images') {
            steps {
                sh '''
                    echo "Building backend image..."
                    docker build -t $BACKEND_IMAGE ./server

                    echo "Building frontend image..."
                    docker build -t $FRONTEND_IMAGE ./client --build-arg VITE_API_URL=http://localhost:5000/api
                '''
            }
        }

        stage ('Run with docker compose') {
            steps {
                sh '''
                    echo "Starting MERN app with docker compose..."
                    docker compose up -d

                    echo "Showing running containers..."
                    docker ps 
                '''
            }
        }
    }
}