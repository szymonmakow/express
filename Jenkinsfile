pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/szymonmakow/express.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("express-app")
                }
            }
        }
    }
}
