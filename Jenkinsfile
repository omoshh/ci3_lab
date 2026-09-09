pipeline {
    agent any
    tools { nodejs 'node16' }

    environment {
        CI        = 'true'
        IMAGE     = "${env.BRANCH_NAME == 'main' ? 'nodemain:v1.0' : 'nodedev:v1.0'}"
        PORT      = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        CONTAINER = "${env.BRANCH_NAME == 'main' ? 'app-main' : 'app-dev'}"
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps { sh 'npm install' }
        }
        stage('Test') {
            steps { sh 'npm test' }
        }
        stage('Build docker image') {
            steps { sh "docker build -t ${IMAGE} ." }
        }
        stage('Deploy') {
            steps {
                sh "docker rm -f ${CONTAINER} || true"
                sh "docker run -d --name ${CONTAINER} --expose ${PORT} -p ${PORT}:3000 ${IMAGE}"
            }
        }
    }
}
