pipeline {
    agent any

    stages {

      stage('Clone') {
    steps {
        git branch: 'main',
            url: 'https://github.com/srikar-vidya/jenkins-devops-project.git'
    }
}

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t srikarvidya/js-multistage:v1 .'
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
                    sh 'docker push srikarvidya/js-multistage:v1'
                }
            }
        }
    }
}
