pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                dir('express') {
                    script {
                        docker.build("express-app")
                    }
                }
            }
        }
    }
}
