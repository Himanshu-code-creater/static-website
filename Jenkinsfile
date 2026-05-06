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
                git branch: '*/master',
                git 'https://github.com/Himanshu-code-creater/static-website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Test') {
          steps {
              sh 'docker run -d -p 8081:80 --rm $IMAGE_NAME:$TAG && sleep 3 && curl -f localhost:8081'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker rm -f $CONTAINER_NAME || true'
                sh 'docker run -d -p 80:80 --name $CONTAINER_NAME $IMAGE_NAME:$TAG'
            }
        }
    }
}
