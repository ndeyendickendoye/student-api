pipeline {
    agent any
    tools {
        maven 'Maven-3.9'
        jdk   'JDK-17'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }
        stage('Tests') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    // Utilisation de la classe interne pour éviter l'erreur "NoSuchMethodError"
                    step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/*.xml'])
                }
            }
        }
        stage('Archive') {
            steps {
                // Utilisation de la classe interne pour l'archivage
                step([$class: 'ArtifactArchiver', artifacts: 'target/*.jar', fingerprint: true])
            }
        }
    }
    post {
        success { echo 'Pipeline reussi avec succes !' }
        failure { echo 'Pipeline echoue -- consultez les logs.' }
    }
}
