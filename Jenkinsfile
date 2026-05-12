pipeline {
    agent {
        node {
            label 'dockerhost-build-server'
        }
    }

    tools {
        maven 'maven-3.9.6'
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
                echo 'Moving .war file to root...'
                sh 'mv target/*.war .'
            }
        }

        stage('cleanup') {
            steps {
                echo 'Cleaning up old containers...'
                sh 'docker system prune -a --volumes --force --filter "label=devops-web-project-server"'
            }
        }

        stage('build image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t darkisma/devops-web-project:v1 --label devops-web-project-server .'
            }
        }

        stage('run container') {
            steps {
                echo 'Starting container...'
                sh 'docker run -d --name devops-web-project-server --label devops-web-project-server -p 8081:8080 darkisma/devops-web-project:v1'
            }
        }
    }
}
