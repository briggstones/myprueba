pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/briggstones/myprueba.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Code Analysis') {
            steps {
                // Aquí puedes integrar herramientas de análisis como SonarQube
                echo 'Performing code analysis...'
            }
        }
        stage('Deploy') {
            steps {
                // Aquí puedes agregar pasos de despliegue
                echo 'Deploying application...'
            }
        }
    }
}