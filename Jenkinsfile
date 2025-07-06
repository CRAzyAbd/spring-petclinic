pipeline {
    agent any

    tools {
        maven 'Maven 3.8.1'
    }

    environment {
        IMAGE_NAME = 'petclinic-app'
        CONTAINER_NAME = 'petclinic-container'
        HOST_PORT = '9090'
        CONTAINER_PORT = '8080'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/CRAzyAbd/spring-petclinic.git'
            }
        }

        stage('Build App') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t $IMAGE_NAME .'
            }
        }

        stage('Remove Old Container') {
            steps {
                script {
                    sh 'sudo docker stop $CONTAINER_NAME || true'
                    sh 'sudo docker rm $CONTAINER_NAME || true'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'sudo docker run -d -p $HOST_PORT:$CONTAINER_PORT --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo 'App deployed inside Docker successfully!'
        }
        failure {
            echo 'Something went wrong.'
        }
    }
}

