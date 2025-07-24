pipeline {
    agent any

    tools {
        nodejs "NodeJs24" // Configura una instalación de Node.js en Jenkins
        dockerTool 'dockertools'  // Cambia el nombre de la herramienta según tu configuración en Jenkins
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                sh 'npm install'
            }
        }

        sstage('Ejecutar Tests') {
            steps {
                sh 'chmod +x ./node_modules/.bin/jest'  // Soluciona el problema de permisos
                sh 'npm test -- --ci --runInBand'
            }
        }

        stage('Construir Imagen Docker') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
            }
        }

        stage('Ejecutar Contenedor Node.js') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                sh '''
                    docker stop hola-mundo-node || true
                    docker rm hola-mundo-node || true
                    docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                '''
            }
        }
    }
}