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
                    dockerImage = docker.build("express-app")
                }
            }
        }

        stage('Test in Container') {
            steps {
                script {
                    docker.image("express-app").inside {
                        sh 'npm test > test-results.txt'
                    }
                }
            }
        }

        stage('Artifacts') {
            steps {
                archiveArtifacts artifacts: 'test-results.txt', fingerprint: true
            }
        }
    }
}
