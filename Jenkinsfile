pipeline {
    agent any
    environment {
        DOCKER_HUB_USER = 'max4245'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MiAPP231/microservices-app.git'
            }
        }
        stage('Build and Push User Service') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        def image = docker.build("${DOCKER_HUB_USER}/user-service:${BUILD_NUMBER}", "./user-service")
                        image.push()
                        image.push("latest")
                    }
                }
            }
        }
        stage('Build and Push Order Service') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        def image = docker.build("${DOCKER_HUB_USER}/order-service:${BUILD_NUMBER}", "./order-service")
                        image.push()
                        image.push("latest")
                    }
                }
            }
        }
        stage('Build and Push Gateway') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        def image = docker.build("${DOCKER_HUB_USER}/gateway:${BUILD_NUMBER}", "./gateway")
                        image.push()
                        image.push("latest")
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Все образы успешно собраны и отправлены в Docker Hub!'
        }
        failure {
            echo 'Ошибка сборки!'
        }
    }
}