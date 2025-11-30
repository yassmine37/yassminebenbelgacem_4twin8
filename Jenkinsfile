pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = docker-hub-creds
        DOCKER_HUB_REPO = 'yassmine37/student-management'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/yassmine37/yassminebenbelgacem_4twin8.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test Docker') {      // <-- Nouvelle étape
            steps {
                sh 'docker --version && docker ps'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_HUB_REPO}:${IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        echo $PASSWORD | docker login -u $USERNAME --password-stdin
                        docker push ${DOCKER_HUB_REPO}:${IMAGE_TAG}
                        docker logout
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès ! 🎉'
        }
        failure {
            echo 'Pipeline échoué. ❌'
        }
    }
}
