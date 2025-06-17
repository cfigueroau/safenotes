pipeline {
    agent any

    tools {
        'OWASP_DC_CLI'
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
                bat 'mkdir dependency-check-report'
                dependencyCheck odcInstallation: 'OWASP_DC_CLI',
                    additionalArguments: '--project "safenotes" --scan "." --format "HTML" --format "XML" --out "dependency-check-report" --enableExperimental'
            }
        }

        stage('Publicar Reporte') {
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
