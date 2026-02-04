pipeline {
    agent { label 'slave' }

    environment {
        // Define environment variables here
        DOCKERFILE_CREDS_ID = 'dockerhub-id'
        DEPLOY_IMAGE = 'vlad2030/node-app:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning...'
                // Add your build steps here
                git branch: 'master', url: 'https://github.com/vladsandbox/for-step-2.git'
            }
        }
        stage('Build docker image') {
            steps {
                echo 'Building...'
                // Add your build steps here
                script {
                    docker.build(DEPLOY_IMAGE)
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                script {
                    docker.image(DEPLOY_IMAGE).inside('--entrypoint=') {
                        sh 'npm install'
                        sh 'npm test'
                    }
                }
            }
            post {
                failure {
                    echo 'Tests failed'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add your deploy steps here
                script {
                    docker.withRegistry('', DOCKERFILE_CREDS_ID) {
                        docker.image(DEPLOY_IMAGE).push()
                    }
                }
            }
        }
    }
}