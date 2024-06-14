pipeline {
    agent any

    stages {
        stage('Clonar el Repositorio') {
            steps {
                // Clona el repositorio desde GitHub
                git branch: 'main', url: 'https://github.com/nickend12/escalamiento-master-main.git'
            }
        }
        
        stage('Construir imagen de Docker') {
            steps {
                script {
                    // Construye la imagen de Docker usando la URL de MongoDB como argumento
                    withCredentials([
                        string(credentialsId: 'MONGO_URI', variable: 'MONGO_URI')
                    ]) {
                        docker.build('proyectos-micro:v1', '--build-arg MONGO_URI=${MONGO_URI} .')
                    }
                }
            }
        }
        
        stage('Desplegar contenedores Docker') {
            steps {
                script {
                    // Despliega los contenedores Docker usando docker-compose
                    withCredentials([
                        string(credentialsId: 'MONGO_URI', variable: 'MONGO_URI')
                    ]) {
                        sh 'docker compose up -d'
                    }
                }
            }
        }
    }

    post {
        always {
            // Envia un correo electrónico con el estado del build
            emailext (
                subject: "Status del build: ${currentBuild.currentResult}",
                body: "Se ha completado el build. Puede detallar en: ${env.BUILD_URL}",
                to: "sey7046@gmail.com",
                from: "jenkins@iudigital.edu.co"
            )
        }
    }
}
