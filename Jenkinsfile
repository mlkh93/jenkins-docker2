pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('mlkh93')
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t melydia/test -t mlkh93/jenkins-docker2 .'
            }
        }
        stage('Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'mlkh93', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                }
            }
        }
        stage('Push') {
            steps {
                sh 'docker push melydia/test'
                sh 'docker push mlkh93/jenkins-docker2'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}

