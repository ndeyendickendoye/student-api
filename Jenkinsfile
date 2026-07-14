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
                // 'mvn clean verify' compile, teste et génère les rapports JaCoCo
                bat 'mvn clean verify'
            }
            post {
                always {
                    // Publication des résultats JUnit
                    step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/*.xml'])
                    // Publication du rapport de couverture JaCoCo
                    step([$class: 'JacocoPublisher', execPattern: 'target/jacoco.exec', classPattern: 'target/classes', sourcePattern: 'src/main/java'])
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
            echo 'Pipeline reussi avec succes ! JaCoCo et JUnit generes.' 
        }
        failure { 
            echo 'Pipeline echoue -- consultez les logs du Build.' 
        }
    }
}
