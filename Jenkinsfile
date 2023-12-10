pipeline {

    agent any

    stages {
        stage ('Clinar repositorio'){
            steps {
                git branch: 'master', credentialsId: 'git-jenkins', url: 'https://github.com/maicolvc88/proyecto_balanceado.git'
            }
        }
       stage('Construir image de docker'){
            steps {
                    script {
                        withCredentials([
                            string(credentialsId: 'MONGO_URI', variable: 'MONGO_URI')
                        ]) {
                            sh """
                            docker build --build-arg MONGO_URI=${MONGO_URI} -t proyectos-micros:v1 .
                            """  
                        }
                    }
                }
                steps {
                    script {
                        withCredentials([
                            string(credentialsId: 'MONGO_URI', variable: 'MONGO_URI')
                        ]) {
                            sh """
                            docker build --build-arg MONGO_URI=${MONGO_URI} -t docker-compose.yml .
                            """  
                        }
                    }
                }
                steps {
                    script {
                        withCredentials([
                            string(credentialsId: 'MONGO_URI', variable: 'MONGO_URI')
                        ]) {
                            sh """
                            docker build --build-arg MONGO_URI=${MONGO_URI} -t loader-balancer .
                            """  
                        }
                    }
                }
        }
        stage('Desplegar contenedor docker'){
            steps {
                script {
                    withCredentials([
                            string(credentialsId: 'MONGO_URI', variable: 'MONGO_URI')
                    ]) {
                        sh """
                            docker-compose -f docker-compose.yml up -d
                        """
                    }
                }
            }
        }
    }
}
