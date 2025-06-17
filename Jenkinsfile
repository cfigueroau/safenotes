pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/cfigueroau/safenotes.git'
            }
        }

        stage('Instalar dependencias') {
            steps {
                bat 'npm install'
            }
        }

        stage('Análisis de dependencias') {
            steps {
                bat 'mkdir dependency-check-report'
                bat '''
                    dependency-check.bat ^
                    --project "SafeNotes" ^
                    --scan "." ^
                    --format "HTML" ^
                    --out "dependency-check-report"
                '''
            }
        }

        stage('Publicar reporte OWASP') {
            steps {
                publishHTML(target: [
                    reportDir: 'dependency-check-report',
                    reportFiles: 'dependency-check-report.html',
                    reportName: 'Reporte de Dependencias OWASP'
                ])
            }
        }
    }
}
