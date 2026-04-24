pipeline {
    agent any
    tools {
        nodejs 'node'
    }
    environment {
        // Если ветка dev - порт 3001 и имя nodedev. Иначе - 3000 и nodemain
        APP_PORT = "${env.BRANCH_NAME == 'dev' ? '3001' : '3000'}"
        IMAGE_NAME = "${env.BRANCH_NAME == 'dev' ? 'nodedev:v1.0' : 'nodemain:v1.0'}"
        CONTAINER_NAME = "${env.BRANCH_NAME}_app"
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build --network=host -t ${IMAGE_NAME} ."
            }
        }
        stage('Deploy') {
            steps {
                // Удаляем старый контейнер, если он был (чтобы не было конфликта портов)
                sh "docker rm -f ${CONTAINER_NAME} || true"
                // Запускаем новый
                sh "docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:3000 ${IMAGE_NAME}"
            }
        }
    }
}
