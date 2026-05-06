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
                docker run -d -p 8081:80 --name test-container $IMAGE_NAME:$TAG
                sleep 5
                curl -f http://localhost:8081
                docker rm -f test-container
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
