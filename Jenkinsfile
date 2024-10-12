pipeline {
    agent any

    tools {
        maven 'Maven 3.8.4'
        jdk 'JDK 11'
    }

    environment {
        GITHUB_REPO = 'https://github.com/briggstones/myprueba.git'
        SONAR_PROJECT_KEY = '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'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GITHUB_REPO}"
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "mvn sonar:sonar -Dsonar.projectKey=${SONAR_PROJECT_KEY}"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                // Aquí irían los comandos para desplegar en un entorno de staging
                // Por ejemplo:
                // sh 'scp target/*.jar user@staging-server:/path/to/deployment/'
            }
        }

        stage('Integration Tests') {
            steps {
                echo 'Running integration tests...'
                // Aquí irían los comandos para ejecutar pruebas de integración
                // Por ejemplo:
                // sh 'mvn integration-test'
            }
        }

        stage('Deploy to Production') {
            when {
                expression { 
                    return env.GIT_BRANCH == 'origin/main'
                }
            }
            steps {
                input message: 'Deploy to production?', ok: 'Deploy'
                echo 'Deploying to production environment...'
                // Aquí irían los comandos para desplegar en producción
                // Por ejemplo:
                // sh 'ansible-playbook -i inventory/production deploy.yml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
            // Puedes agregar notificaciones aquí, por ejemplo:
            // slackSend channel: '#devops-notifications', color: 'good', message: "Pipeline for ${env.JOB_NAME} completed successfully."
        }
        failure {
            echo 'Pipeline failed :('
            // Notificaciones de fallo:
            // slackSend channel: '#devops-notifications', color: 'danger', message: "Pipeline for ${env.JOB_NAME} failed. Please check the logs."
        }
        always {
            echo 'Cleaning up workspace...'
            deleteDir()
        }
    }
}


