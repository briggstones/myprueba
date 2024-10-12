pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Clonar el repositorio desde Git
                git 'https://github.com/briggstones/myprueba.git'
            }
        }
        stage('Build') {
            steps {
                // Usar Maven para construir el proyecto
                bat 'mvn clean install'
            }
        }
        stage('Code Analysis') {
            steps {
                // Comandos para análisis de código
                // Aquí puedes usar herramientas como SonarQube
                bat 'mvn sonar:sonar'
            }
        }
        stage('Deploy') {
            steps {
                // Comando para copiar el artefacto al servidor de despliegue
                // Asegúrate de que el usuario tenga permisos adecuados
                bat 'copy target\\simple-project-1.0-SNAPSHOT.jar \\\\192.168.1.129\\C$\\Deployments\\'
            }
        }
    }
}
