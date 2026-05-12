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

        stage('Update Kubernetes Manifest') {
            steps {

                sh """
                sed -i 's#image:.*#image: srikarvidya/js-multistage:${IMAGE_TAG}#' deployment.yaml
                """

                sh 'git config --global user.email "jenkins@example.com"'
                sh 'git config --global user.name "jenkins"'

                sh 'git add deployment.yaml'

                sh 'git commit -m "Updated image to ${IMAGE_TAG}" || true'

                withCredentials([usernamePassword(
                    credentialsId: 'github',
                    usernameVariable: 'GUSER',
                    passwordVariable: 'GPASS'
                )]) {

                    sh 'git push https://${GUSER}:${GPASS}@github.com/srikar-vidya/jenkins-devops-project.git HEAD:main'
                }
            }
        }
    }
}
