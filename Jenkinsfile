pipeline {
    agent any

    environment {
        DEPLOY_DIR = "/home/jho4n/prueba-docker"
        BRANCH = "main"
    }

    stages {

        stage('Verify environment') {
            steps {
                sh '''
                    echo "Verificando herramientas.1212.."
                    docker --version
                    docker-compose --version
                    git --version
                    ls -la $DEPLOY_DIR
                '''
            }
        }

        stage('Update code') {
            steps {
                sh '''
                    echo "Actualizando código..."
                    git config --global --add safe.directory $DEPLOY_DIR
                    cd $DEPLOY_DIR
                    git fetch origin
                    git reset --hard origin/$BRANCH
                '''
            }
        }

        stage('Build and deploy') {
            steps {
                sh '''
                    echo "Construyendo y desplegando..."
                    cd $DEPLOY_DIR
                    docker-compose down || true
                    docker-compose up -d --build
                '''
            }
        }

        stage('Show status') {
            steps {
                sh '''
                    echo "Estado de contenedores:"
                    docker ps
                '''
            }
        }
    }

    post {
        success {
            echo 'Deploy completado correctamente'
        }
        failure {
            echo 'El pipeline falló'
        }
    }
}