pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'   
        maven 'M2_HOME'   

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yassmine37/yassminebenbelgacem_4twin8.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn compile'
            }
        }
    }

    post {
        success {
            echo 'Build Maven réussi '
        }
        failure {
            echo 'Le build Maven a échoué '
        }
    }
}
