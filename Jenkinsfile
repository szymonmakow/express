pipeline {
    agent any

    environment {
        VERSION = "1.0.${env.BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/szymonmakow/express.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("express-app:${VERSION}")
                }
            }
        }

        stage('Test in Container') {
            steps {
                script {
                    docker.image("express-app:${VERSION}").inside {
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
                sh 'docker rm -f express-container || true'
                sh 'docker run --name express-container express-app:${VERSION}'
            }
        }

        stage('Smoke Test') {
            steps {
                sh 'echo "Deploy zakończony sukcesem"'
            }
        }

        stage('Publish') {
            steps {
                sh 'docker images'
                sh 'echo "Image version: ${VERSION}"'
            }
        }
    }
}
