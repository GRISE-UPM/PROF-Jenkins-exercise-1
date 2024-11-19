pipeline {
    agent any

    environment {
        GRADLE_HOME = '/usr/local/gradle'
        PATH = "${GRADLE_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'java -version'
            }
        }
        
        stage('Build') {
            steps {
                // Asegurar permisos de ejecución
                sh 'chmod +x gradlew'
                sh './gradlew build'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Coverage') {
            steps {
                sh './gradlew jacocoTestReport'
            }
        }

        stage('Verify Coverage') {
            steps {
                sh './gradlew check'
            }
        }
    }

    post {
        always {
            junit '**/build/test-results/**/*.xml'
            jacoco execPattern: '**/build/jacoco/*.exec', classPattern: '**/build/classes/java/main', sourcePattern: 'src/main/java'
        }
        failure {
            echo 'Build failed!'
        }
        success {
            echo 'Build succeeded!'
        }
    }
}
