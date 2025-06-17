pipeline {
    agent any

    environment {
        REPORT_DIR = 'dependency-check-report'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git 'https://github.com/cfigueroau/safenotes.git'
            }
        }

        stage('Instalar dependencias') {
            steps {
                bat 'npm install'
            }
        }

        stage('Análisis de Dependencias') {
            steps {
                bat "mkdir ${env.REPORT_DIR}"
                bat """dependency-check.bat ^
                    --project "SafeNotes" ^
                    --scan "." ^
                    --format "HTML" ^
                    --format "XML" ^
                    --out "${env.REPORT_DIR}" ^
                    --enableExperimental"""
            }
        }

        stage('Publicar Reporte') {
            steps {
                publishHTML([
                    reportDir: "${env.REPORT_DIR}",
                    reportFiles: 'dependency-check-report.html',
                    reportName: 'Reporte de Dependencias OWASP'
                ])
            }
        }
    }
}
