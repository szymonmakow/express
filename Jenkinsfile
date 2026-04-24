pipeline {
    agent any

    environment {
        VERSION  = "1.0.${env.BUILD_NUMBER}"
        REGISTRY = "szymonmakow/express-app"
        CONTAINER_NAME = "express-container"
    }

    stages {

        stage('Clean') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    app = docker.build("${REGISTRY}:${VERSION}", "--no-cache .")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image("${REGISTRY}:${VERSION}").inside {
                        sh 'npm install'
                        sh 'npm test > test-results.txt || true'
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
                sh "docker rm -f ${CONTAINER_NAME} || true"
                sh "docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${REGISTRY}:${VERSION}"
            }
        }

        stage('Smoke Test') {
            steps {
                script {
                    sh '''
                    for i in {1..10}; do
                        curl -f http://localhost:3000 && exit 0
                        echo "czekanie na start aplikacji"
                        sleep 2
                    done
                    echo "Smoke test FAILED"
                    exit 1
                    '''
                }
            }
        }

        stage('Publish') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        app.push()
                        app.push('latest')
                    }
                }
                echo "opublikowano obraz: ${REGISTRY}:${VERSION}"
            }
        }
    }

    post {
        always {
            sh "docker rm -f ${CONTAINER_NAME} || true"
            sh "docker rmi ${REGISTRY}:${VERSION} || true"
        }
        success {
            echo "obraz gotowy do użycia: ${REGISTRY}:${VERSION}"
        }
        failure {
            echo "pipeline nie przeszedł poprawnie"
        }
    }
}
