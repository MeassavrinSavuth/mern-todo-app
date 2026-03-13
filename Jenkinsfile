pipeline {

    agent any

    environment {
        DOCKER_HUB_USER = 'meas007'
        IMAGE_NAME = 'mern-todo-app'
        DOCKER_HUB_CREDS = 'docker-hub-credentials'
    }

    stages {

        stage('Build') {
            steps {
                echo 'Installing dependencies and building app...'

                dir('TODO/todo_backend') {
                    sh 'npm install'
                }

                dir('TODO/todo_frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Containerize') {
            steps {
                echo 'Building Docker image...'

                dir('TODO') {
                    sh "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Push') {
            steps {
                echo 'Logging into DockerHub and pushing image...'

                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_HUB_CREDS}",
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                    sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Running container...'

                sh "docker run -d -p 8080:5000 ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest || true"
            }
        }
    }

    post {
        always {
            echo 'Cleaning up local Docker image...'
            sh "docker rmi ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest || true"
        }
    }
}