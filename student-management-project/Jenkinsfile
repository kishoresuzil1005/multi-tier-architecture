pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds'
        DOCKER_IMAGE_BACKEND = 'kishoresuzil/backend'
        DOCKER_IMAGE_FRONTEND = 'kishoresuzil/frontend'
    }

    stages {

       

	stage('Build Docker Images') {
    		steps {
        	dir('student-management-project') {
            sh 'docker build -t kishoresuzil/backend ./backend'
            sh 'docker build -t kishoresuzil/frontend ./frontend'
        }
    }
}


        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Push Images to DockerHub') {
            steps {
                sh 'docker push $DOCKER_IMAGE_BACKEND'
                sh 'docker push $DOCKER_IMAGE_FRONTEND'
            }
        }

        stage('Deploy with Docker Compose') {
    steps {
        dir('student-management-project') {
            sh 'docker compose -f docker-compose.yml down || true'
            sh 'docker compose -f docker-compose.yml up -d --build'
        }
    }
}

    }
}

