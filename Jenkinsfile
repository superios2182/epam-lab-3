pipeline {
    agent any
    environment {
        NODEJS_INSTALLATION = 'node'
        registry = "M3RS/lab-3-cicd-pipeline"
        registryCredential = 'docker_hub_pw'
        dockerImage = ''
    }
    tools {
        nodejs NODEJS_INSTALLATION
    }
    stages {
        stage ('Build') {
            steps {
                sh '''
                   npm install
                   npm build
                   '''
            }
        }
        stage ('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage ('Docker build') {
            steps {
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
        }
        stage ('Deploy') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Clean up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}