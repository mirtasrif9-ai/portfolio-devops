pipeline {
    agent any
    
    triggers {
    pollSCM('H/5 * * * *')
}

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = 'tasrif9/portfolio-devops'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Tag') {
            steps {
                sh 'docker tag $IMAGE_NAME:latest $IMAGE_NAME:build-$BUILD_NUMBER'
            }
        }

        stage('Docker Hub Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
                sh 'docker push $IMAGE_NAME:build-$BUILD_NUMBER'
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
