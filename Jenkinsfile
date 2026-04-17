pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/expressjs/express.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build("express-app")
                }
            }
        }
    }
}
