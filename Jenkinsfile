pipeline {
    agent any

    environment {
        IMAGE_NAME = "madhu58/project"
        IMAGE_TAG = "latest"
    }

    stages {

        // No need to checkout again because Pipeline from SCM
        // already performs "Checkout SCM"
        stage('Build Docker') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat """
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    """
                }
            }
        }

        stage('Push Image') {
            steps {
                bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
            }
        }

        stage('Deploy') {
            steps {
                bat """
                docker stop project || exit 0
                docker rm project || exit 0
                docker run -d --name project -p 80:80 %IMAGE_NAME%:%IMAGE_TAG%
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}