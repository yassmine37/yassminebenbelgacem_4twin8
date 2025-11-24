pipeline {
    agent any

    environment {
        // Credentials Jenkins pour Docker Hub
        DOCKER_HUB_CREDENTIALS = 'docker-hub-creds'  
        // Nom complet de ton repo Docker Hub (username/repo)
        DOCKER_HUB_REPO = 'yassmine37/student-management'  
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupération du code depuis GitHub
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/yassmine37/yassminebenbelgacem_4twin8.git'
            }
        }

        stage('Build Maven') {
            steps {
                // Compilation du projet Java
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Construction de l'image Docker
                sh "docker build -t ${DOCKER_HUB_REPO}:${IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                // Login Docker et push sécurisé via les credentials Jenkins
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        sh "docker push ${DOCKER_HUB_REPO}:${IMAGE_TAG}"
                    }
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
