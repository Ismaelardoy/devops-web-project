pipeline {
    agent {
        node {
            // Este label debe coincidir con el nombre de tu nodo/agente en Jenkins [cite: 303]
            label 'dockerhost-build-server'
        }
    }

    tools {
        // Asegúrate de que este nombre sea igual al configurado en Global Tool Configuration [cite: 307]
        maven 'maven-3.9.6'
    }

    stages {
        stage('Packaging') {
            steps {
                echo 'Packaging application...' [cite: 312]
                sh 'mvn clean package' [cite: 313]
            }
        }

        stage('Copying war file') {
            steps {
                echo 'Moving .war file to root directory...' [cite: 318]
                // Mueve el archivo compilado para que el Dockerfile lo encuentre [cite: 319]
                sh 'mv target/*.war .' 
            }
        }

        stage('cleanup') {
            steps {
                echo 'Cleaning up old images and containers...' [cite: 322]
                // Borra contenedores previos con la misma etiqueta para evitar conflictos [cite: 324]
                sh 'docker system prune -a --volumes --force --filter "label=devops-web-project-server"'
            }
        }

        stage('build image') {
            steps {
                echo 'Building Docker image...' [cite: 328]
                // REEMPLAZA TU_USUARIO_DOCKERHUB con tu usuario real [cite: 330]
                sh 'docker build -t darkisma/devops-web-project:v1 --label devops-web-project-server .'
            }
        }

        stage('run container') {
            steps {
                echo 'Starting container...' [cite: 334]
                // Ejecuta el contenedor mapeando el puerto 8081 al 8080 del Tomcat [cite: 336, 337]
                sh 'docker run -d --name devops-web-project-server --label devops-web-project-server -p 8081:8080 darkisma/devops-web-project:v1'
            }
        }
    }
}
