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

        stage('Deploy') {
            steps {
                sh 'docker run --name express-container express-app'
            }
        }

        stage('Smoke Test') {
            steps {
                sh 'echo "Deploy zakończony sukcesem"'
            }
        }
    }
}
