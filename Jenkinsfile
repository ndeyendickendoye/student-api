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
        stage('Build & Test') {
            steps {
                // Compilation et exécution des tests sans JaCoCo
                bat 'mvn clean package'
            }
            post {
                always {
                    // Publication des résultats JUnit
                    step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/*.xml'])
                }
            }
        }
        stage('Archive') {
            steps {
                // Archivage du fichier .jar produit
                step([$class: 'ArtifactArchiver', artifacts: 'target/*.jar', fingerprint: true])
            }
        }
    }
    post {
        success { 
            echo 'Pipeline reussi avec succes !' 
        }
        failure { 
            echo 'Pipeline echoue -- consultez les logs.' 
        }
    }
}
