pipeline {

    agent any

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/srikar-vidya/jenkins-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t srikarvidya/js-multistage:${IMAGE_TAG} .'
            }
        }

        stage('Push Docker Image') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'

                    sh 'docker push srikarvidya/js-multistage:${IMAGE_TAG}'
                }
            }
        }
    }
}
