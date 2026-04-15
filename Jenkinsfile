pipeline {
    agent {
        docker { image 'node:18' }
    }
    stages {
        stage('Install') {
            steps {
                echo 'Instalacja zależności'
                sh 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Uruchamianie testów'
                sh 'npm test'
            }
        }
    }
    post {
        success {
            echo 'Wszystkie testy przeszły'
        }
        failure {
            echo 'Testy nie przeszły'
        }
    }
}
