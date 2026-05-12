pipeline {
    agent {
        node {
            label 'dockerhost-build-server'
        }
    }

    tools {
        maven 'maven-3.9.6'
    }

    environment {
        // Asegúrate de que este ID coincida con el que creaste en Jenkins -> Credentials
        DOCKERHUB_CREDENTIALS = credentials('devops-dockerhub-token')
    }

    stages {
        stage('Packaging') {
            steps {
                echo 'Packaging application...'
                sh 'mvn clean package'
            }
        }

        stage('Copying war file') {
            steps {
                echo 'Moving war file to root...'
                sh 'mv target/*.war .'
            }
        }

        stage('build image') {
            steps {
                echo 'Building Docker image...'
                // Añadido tu usuario darkisma para que sea válida para Docker Hub
                sh 'docker build -t darkisma/devops-web-project:v1 --label devops-webproject-server .'
            }
        }

        stage('Docker Hub Login') {
            steps {
                echo 'Logging into Docker Hub...'
                // Comando corregido en una sola línea
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing image to Docker Hub...'
                sh 'docker push darkisma/devops-web-project:v1'
            }
        }
    }

    post {
        always {
            echo 'Logging out from Docker Hub...'
            sh 'docker logout'
        }
    }
}
