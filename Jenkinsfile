pipeline {
    agent any

    tools {
        nodejs "Node24"
        dockerTool "Dockertool" 
    }

    stages {
        stage('Instalar dependencias') {
    steps {
        // Instalamos la librería faltante usando el usuario root
        sh 'apt-get update && apt-get install -y libatomic1'
        sh 'npm install'
    }
}

        stage('Ejecutar tests') {
    steps {
        // Otorgamos permisos de ejecución a todos los binarios de node_modules
        sh 'chmod +x ./node_modules/.bin/jest'
        sh 'npm test'
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
 