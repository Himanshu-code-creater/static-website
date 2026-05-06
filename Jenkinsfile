pipeline {
    agent any

    environment {
        IMAGE_NAME = "nginx"
        TAG = "latest"
        CONTAINER_NAME = "nginx-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Himanshu-code-creater/static-website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    docker run -d -p 8081:80 --name test-container nginx:latest
                    sleep 5
                    CONTAINER_IP=$(docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' test-container)
                    curl -f http://$CONTAINER_IP:80
                    docker stop test-container
                    docker rm test-container
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d -p 80:80 --name $CONTAINER_NAME $IMAGE_NAME:$TAG
                '''
            }
        }
    }
}
