pipeline {
    agent {
        docker { image 'maven:3.9.4-openjdk-17' }
    }
    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials'
        IMAGE_NAME = 'ayagrami/tp2devops'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git branch: 'main', url: 'https://github.com/ayagrami/DevOpsTP2.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Docker') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Push Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
