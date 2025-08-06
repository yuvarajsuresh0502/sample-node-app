pipeline {
    agent any

    environment {
        IMAGE_NAME = 'sample-node-app'
        DOCKERHUB_USER = 'yuvaraj3546'
    }

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/yuvarajsuresh0502/sample-node-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/$IMAGE_NAME:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push $DOCKERHUB_USER/$IMAGE_NAME:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker rm -f sample-app || true'
                sh 'docker run -d -p 3000:3000 --name sample-app $DOCKERHUB_USER/$IMAGE_NAME:latest'
            }
        }
    }

    post {
        success {
            echo '✅ App deployed successfully!'
        }
        failure {
            echo '❌ Build failed.'
        }
    }
}
